<properties 
    pageTitle="A többszörös-DRM és a hozzáférés-vezérlés CENC: egy hivatkozás tervezés és a végrehajtás Azure és Azure Media Services |} Microsoft Azure" 
    description="További tudnivalók a Microsoft® zökkenőmentes Streaming ügyfél portolása Kit licencelési." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>A többszörös-DRM és a hozzáférés-vezérlés CENC: egy hivatkozás tervezés és a végrehajtás Azure és Azure Media Services

##<a name="key-words"></a>Kulcsszavak
 
Azure Active Directory, az Azure Media Services Azure Media Player, dinamikus titkosítást, licenc kézbesítési, PlayReady, Widevine, FairPlay, közös Encryption(CENC), többszintű-DRM, Axinom, kötőjel, EME, MSE, JSON webes jogkivonat (JWT), Jogcímeken, Modern böngészők, kulcs átváltási, szimmetrikus kulcsot aszimmetrikus kulcsot, OpenID csatlakozni, X509 tanúsítvány. 

##<a name="in-this-article"></a>Ebben a cikkben

Ez a cikk az alábbi témakörök közül:

- [– Bevezetés](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Ez a cikk áttekintése](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Egy hivatkozás Tervező](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Tervező megfeleltetése végrehajtásához technológia](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Végrehajtása](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Végrehajtási eljárások](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Néhány meglátások végrehajtása](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [További témakörök végrehajtása](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [Http- vagy HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory fő átváltási aláírása](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Hol található a hozzáférési jogkivonat?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Mit kell tudni élő adatfolyam?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Mit kell tudni az Azure Media Services kívüli licenc kiszolgálók?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Mi történik, ha használni kívánt egy egyéni STS?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [A kész rendszer és tesztelése](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Felhasználói bejelentkezés](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Titkosított Media bővítmények használata PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Widevine EME verzióval](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Nem jogosult felhasználók](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Egyéni biztonságos jogkivonat szolgáltatást futtató](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Összefoglalás](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>– Bevezetés

Érdemes a jól ismert, hogy az tervezhet és készíthet egy DRM alrendszer az OTT az összetett tevékenység vagy online a folyamatos átvitelű megoldás. És ebben a részben speciális DRM szolgáltatók kihelyező operátorok online videó szolgáltatók közös ajánlott. A dokumentum célja egy hivatkozás tervezés és a végpontok közötti DRM alrendszer OTT vagy online adatfolyam megoldás végrehajtásának bemutatására.

A célzott olvasókat a dokumentum olyan mérnökök DRM alrendszer OTT vagy online a folyamatos átvitelű/többfázisú-screen megoldásokat vagy bármely DRM alrendszer érdekli olvasók dolgozik. Azon a feltételezésen, hogy olvasók jártas a DRM technológiák forgalomba, például PlayReady, Widevine, FairPlay vagy Adobe Access közül.

DRM azt is (közös titkosítás) CENC a többszörös-DRM. Az online streaming, és OTT iparágban fő trend CENC használata többfázisú-native-DRM ügyfél platformokhoz készült különböző, amely olyan, a SHIFT billentyűt az előző trendjét egy egyetlen DRM és az ügyfél SDK használatával különböző ügyfél platformokon. A csoportos native DRM CENC használatakor PlayReady és a Widevine a [Közös titkosítási (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specifikációja egy titkosított.

A többszörös-DRM CENC előnyeiről a következők:

1. Csökkenti a titkosítási költség, mivel az egy egyetlen titkosítási feldolgozás használják a natív DRMs; a más platformokon célba juttatása
1. Csökkenti a költségeket titkosított eszközök kezelése, mivel a titkosított eszközök csak egyetlen példányt van szükség;
1. Megszünteti a költség licencelése, mivel a natív ügyfele DRM általában ingyenes, saját natív platformon DRM ügyfél.

A Microsoft-aktív hatnak szaggatott és CENC együtt néhány fő iparágban lejátszó volt. Microsoft Azure Media Services támogatja a szaggatott és CENC van már biztosítása. Legutóbbi hirdetmények, olvassa el Mingfei's blogok: [kihirdetése Google Widevine licenc kézbesítési services az Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)és az [Azure Media Services felveszi a Google Widevine összecsomagolása többszörös-DRM adatfolyam kipróbálására](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Ez a cikk áttekintése

A cél, a jelen cikk az alábbiakat tartalmazza:

1. Tartalmaz egy hivatkozást tervezés DRM alrendszer CENC használata több-DRM;
1. A hivatkozás megvalósítás Ez a témakör a Microsoft Azure/Azure Media Services platform;
1. Ismerteti, hogy bizonyos tervezéséhez és kivitelezéséhez témaköröket.

A cikkben "többszörös-DRM" az alábbi foglalja magában:

1. Microsoft PlayReady
1. A Google Widevine
1. Apple FairPlay (Azure Media Services által még nem támogatott)

Az alábbi táblázat összefoglalja a natív platform/natív alkalmazást, és az egyes DRM által támogatott böngészők.

**Ügyfél-Platform**|**Natív DRM támogatás**|**Böngésző/alkalmazás**|**Adatfolyam-formátumok**
----|------|----|----
**Intelligens televízió, OTT vezérléséhez vezérléséhez, operátor**|PlayReady elsősorban, illetve Widevine és/vagy más|Linux, Opera, WebKit, más|Különböző formátumokban
**A Windows 10-eszközökön (Windows rendszerben, a Windows-Táblagépekhez, Windows Phone, Xbox)**|PlayReady|Szegély/IE11/EME MS<br/><br/><br/>UWP|KÖTŐJEL (-HLS, PlayReady nem támogatott)<br/><br/>VONAL, a zökkenőmentes Streaming (az HLS, PlayReady nem támogatott) 
**Android-eszközön (telefonon, Táblagépen TV)**|Widevine|A Chrome/EME|GONDOLATJEL
**iOS (iPhone, iPad), az OS X ügyfelek és az Apple TV**|FairPlay|Safari 8 +/ EME|HLS
**A beépülő modul: Adobe Primetime**|Az Access Primetime|Böngésző beépülő modul|HDS, HLS

Minden egyes DRM telepítésének aktuális állapotát, figyelembe véve szolgáltatás fog általában végre kívánja hajtani, hogy a legegyszerűbben a végpontok típusú cím 2-es vagy 3 DRMs.

A szolgáltatás logika komplexitását és a felhasználói élmény a különböző ügyfélalkalmazásokon bizonyos szintet ügyféloldali bonyolultsága között egy útján van.

A kijelölt nevekre, amelyre érdemes figyelni feledkezzen tétele:

- PlayReady natív módon történik minden egyes Android rendszerű eszközökön és a szoftver SDK gyakorlatilag bármilyen platformon keresztül érhető el a Windows-eszközön
- Minden Android-eszközön, a Chrome-ban és az egyes eszközök esetén Widevine natív módon történik.
- Csak iOS és az ügyfelek a Mac OS, illetve iTunes FairPlay érhető el.

Így egy tipikus több DRM lesz:

- : 1 PlayReady és Widevine
- 2 lehetőség: PlayReady, Widevine és FairPlay


## <a name="a-reference-design"></a>Egy hivatkozás Tervező

Ebben a részben egy hivatkozás tervezés, amely agnostic technológiák végrehajtásához használt bemutatja azt.

Egy DRM alrendszer az alábbi összetevőket tartalmazhatja:

1. Kulcskezelő
1. DRM csomagolása
1. DRM licenc kézbesítési
1. Jogosultság ellenőrzése
1. Hitelesítés és engedélyezése
1. Player
1. Origin/CDN

Az alábbi ábra szemlélteti a magas szintű kapcsolati DRM alrendszer-összetevők között.

![CENC DRM alrendszer](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
A tervező az alábbi három alapvető "réteg":

1. Vissza az office réteget, amelyek nem külső felekkel; megjelenített (a fekete)
1. Összes néző nyilvános; végpontjait tartalmazó "DMZ" réteg (kék)
1. Nyilvános internetkapcsolat (világoskék) tartalmazó réteget CDN és a forgalmat a lejátszó nyilvános interneten keresztül.
 
Tartalomkezelés eszköz DRM-védelem biztonságkezelési kell lennie, függetlenül érdemes statikus vagy dinamikus titkosítást. A ráfordítások DRM titkosításhoz tartalmaznia kell:

1. MBR videotartalmak;
1. Tartalom kulcsot.
1. Licenc WIA URL-ek.

A lejátszás idő alatt a magas szintű folyamat a következő:

1. A hitelesített felhasználó;
1. Jóváhagyási jogkivonat létrejön a felhasználói;
1. DRM védett tartalmat (jegyzék); Player letöltése
1. Player elküld licenc WIA kérését licenc kiszolgálókhoz együtt kulcs Azonosítóját és engedélyezési jogkivonat.

Mielőtt áthelyezése a következő témakör néhány szó a kulcskezelő felépítésének.

**ContentKey – eszköz**|**Eset**
------|---------------------------
1 – a-1|A legegyszerűbb eset. A finomabb vezérlő biztosít. De ez általában ennek hatására a legmagasabb licenc kézbesítési költségét. A minimális egy-egy licencet kérelem szükség, minden védett eszköz.
1 – a-többhöz|Több eszközök használhatja is ugyanazt a tartalom kulcsot. Összes eszközök egy logikai csoport, például műfaj vagy részhalmazát műfaj (vagy a film gén), például egy tartalom kulcs használata sikerült.
Sok – 1|Több tartalom billentyűk minden eszköz van szükség. <br/><br/>Ha például kell alkalmaznia az dinamikus CENC védelem a többszörös-DRM MPEG-vonal- és dinamikus AES-128 titkosítás HLS, szükségességének két külön tartalom kulcsot, a saját ContentKeyType saját. (A tartalom kulcs dinamikus CENC védelmet használt ContentKeyType.CommonEncryption kell használni, a dinamikus AES-128 titkosítást használja, a tartalom billentyűt, ContentKeyType.EnvelopeEncryption kell használni.)<br/><br/>Egy másik példa CENC védelem kötőjel tartalom elméletben egy video-adatfolyam és más tartalom kulcs audio-adatfolyam védelme védelme tartalom kulcs használható. 
Sok – a - többhöz|A fenti két forgatókönyvet kombinációját: tartalom billentyűk az egyik készlete minden azonos eszköz "csoport" több eszközök segítségével.


Egy másik fontos szempontok tényező licencek állandó és állandó e.

Miért fontosak az alábbi szempontok? 

Ha nyilvános felhő licenc kézbesítési használ licenc kézbesítési költség közvetlen gyakorolt hatás rendelkeznek. Nézzünk meg az alábbi két különböző tervezési esetekben mutatja be:



1. Havi előfizetéshez: állandó licenc és az 1-a-többhöz tartalom kulcs tárgyi eszköz megfeleltetés használata. Pl. a gyerekek mozgóképek azt termékkulccsal egyetlen tartalom titkosításának. Ebben az esetben: 

    # Licencek igényelt összes gyerekek filmek/eszközökre készült összes = 1

1. Havi előfizetéshez: állandó licenc, illetve 1-1-megfeleltetés tartalom billentyűt és eszköz között. Ebben az esetben:

    # Licencek igényelt összes gyerekek filmek/eszközökre készült összes [# filmek vizsgált] = [# munkamenetek] x

Hogy könnyen átláthassa, a két különféle formák vonhat nagyon különböző licenc kérelem így mintázatok licenc kézbesítési költséget, ha például az Azure Media Services nyilvános felhő által biztosított licenc kézbesítési szolgáltatásnak.

## <a name="mapping-design-to-technology-for-implementation"></a>Tervező megfeleltetése végrehajtásához technológia

Ezután azt feleltesse meg az általános tervezési Microsoft Azure/Azure Media Services platformon technológiák minden építőelem használandó technológiákat megadásával.

A következő táblázat mutatja a hozzárendelés:

**Építőelem**|**Technológia**
------|-------
**Player**|[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/)
**Identitásszolgáltató (IDP)**|Azure Active Directory
**Jogkivonat-szolgáltatás biztonságos**|Azure Active Directory
**DRM védelem munkafolyamat**|Azure Media Services dinamikus védelme
**DRM licenc kézbesítési**|1. az azure Media Services licenc kézbesítési (PlayReady, Widevine, FairPlay), vagy <br/>2. a Axinom licenc kiszolgáló <br/>3. a egyéni PlayReady licenc kiszolgáló
**Származás**|Azure Media Services-adatfolyam végpont
**Kulcskezelő**|Nem szükséges hivatkozást végrehajtása
**Tartalomkezelés**|A C# console-alkalmazások

Identitás-szolgáltató (IDP) és a biztonságos jogkivonat szolgáltatás (STS) más szóval lesz az Azure Active Directory. A Windows Media player fogjuk használni [Azure Media Player API -val](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services és az Azure Media Player is támogatja a szaggatott és CENC a többszörös-DRM.

Az alábbi ábra mutatja az általános szerkezetének és a fenti technológia hozzárendeléssel továbbításához.

![A AMS CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Annak érdekében, hogy állítsa be a dinamikus CENC titkosítást, a tartalom kezelése eszköz a következő bemenetben fogja használni:

1. Nyissa meg a tartalom;
1. Tartalom kulcs kulcs létrehozása és kezelése;
1. Licenc WIA URL-címek;
1. Azure Active Directory adatait listája.

A kimenet: a tartalom kezelése eszköz a következő lesz:

1. A hogyan licenc kézbesítési JWT jogkivonat és DRM licenc jellemzői; ellenőrzi az beállítások tartalmazó ContentKeyAuthorizationPolicy
1. A folyamatos átvitelű formátumát, DRM-védelem és licenc WIA URL-címek jellemzői tartalmazó AssetDeliveryPolicy.

Során futtatókörnyezet, a folyamat van, mint alább:

1. Felhasználói hitelesítés esetén a JWT jogkivonat jön létre;
1. A jogcímalapú a JWT jogkivonat szereplő egyik "csoportok" felelős "EntitledUserGroup" csoport objektum azonosítója tartalmazó. Ez az állítás használandó áthaladó "jogosultság jelölőnégyzet".
1. Player letöltése ügyfél jegyzék egy CENC a védett tartalmat, és a "" a következőket látja:
    1. Azonosítót, kulcs 
    1. a tartalom védett, CENC
    1. Licenc WIA URL-ek.

1. Player megkeresést licenc WIA támogatott böngésző/DRM alapján. A licenc WIA kérésben főbb azonosító és a JWT jogkivonat is benyújtandó. Licenc kézbesítési szolgáltatás ellenőrzi a JWT jogkivonat, és a szükséges licenc kiadása előtt szereplő követelések.

##<a name="implementation"></a>Végrehajtása

###<a name="implementation-procedures"></a>Végrehajtási eljárások

A végrehajtás tartalmazza az alábbi lépéseket:

1. Felkészülés a próba eszköz(ök): többszörös-átviteli sebesség tesztelése videó kódolását, csomag tördelt MP4 az Azure Media Services. Ez az eszköz nem védett DRM. DRM-védelem később dinamikus védelmet végzi.
1. Kulcs Azonosítóját és tartalmak létrehozása kulcs (tetszés szerint a fő seed). A célra kulcskezelő rendszer nincs szükség, mert azt csak egyetlen csoportja foglalkoznak főbb azonosító és a tartalom kulcs több vizsgált eszközök;
1. AMS API segítségével többszörös-DRM licenc kézbesítési szolgáltatás a próba-eszköz beállítása. Egyéni licenc kiszolgálók használatakor a vállalat vagy az Azure Media Services licenc-szolgáltatások helyett a vállalat szállítók ugorja át ezt a lépést, és adja meg a licenc WIA URL-ek a licenc kézbesítési konfigurációs lépés. Adja meg, bizonyos részletes beállításokat, például engedélyezési házirend-korlátozás, a válasz-sablonokat licenc különböző DRM licenc szolgáltatások stb AMS API van szükség. Ekkor az Azure portálon nem még biztosít a szükséges felhasználói felületének ebben a konfigurációban. API szintű információkat talál, és minta Ágnes Kornich dokumentumban kódot: [PlayReady használatával, illetve Widevine dinamikus közös titkosítást](media-services-protect-with-drm.md). 
1. AMS API segítségével a próba-eszköz eszköz kézbesítési házirend beállítása. API szintű információkat talál, és minta Ágnes Kornich dokumentumban kódot: [PlayReady használatával, illetve Widevine dinamikus közös titkosítást](media-services-protect-with-drm.md).
1. Hozzon létre, és az Azure Active Directory-bérlő konfigurálása Azure;-ban
1. Néhány felhasználói fiókok és csoportok létrehozása az Azure Active Directory-ös bérlőben: legalább kell létrehoznia "EntitledUser" csoport és a felhasználó hozzáadása ehhez a csoporthoz. Felhasználók ebben a csoportban a licenc beszerzési továbbítja jogosultság jelölőnégyzetet, és a csoporthoz nem tartozó felhasználók hitelesítés ellenőrzése átadni sikertelen lesz, és nem tudnak szerezheti be a licencet. Az Azure Active Directory által kibocsátott JWT jogkivonat igényt szükséges "csoportokhoz" a "EntitledUser" csoport tagja. E állítást követelmény konfigurálása a többszörös-DRM licenc kézbesítési szolgáltatások lépést kell megadni.
1. Hozzon létre egy ASP.NET MVC appot, amely a videolejátszó szolgáltatója lesz. ASP.NET az alkalmazás és a felhasználói hitelesítés szemben az Azure Active Directory-bérlői védi. A felhasználói hitelesítés után kapott hozzáférési jogkivonat megfelelő követelések jelennek meg. Ebben a lépésben OpenID csatlakozás API ajánlott. A következő NuGet csomagokban telepítéséhez szükséges:
    - Telepítés-csomag Microsoft.Azure.ActiveDirectory.GraphClient
    - Telepítés-csomag Microsoft.Owin.Security.OpenIdConnect
    - Telepítés-csomag Microsoft.Owin.Security.Cookies
    - Telepítés-csomag Microsoft.Owin.Host.SystemWeb
    - Telepítés-csomag Microsoft.IdentityModel.Clients.ActiveDirectory
1. Hozzon létre egy lejátszó [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/)segítségével. [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) lehetővé teszi, hogy melyik DRM technológia használata különböző DRM platformon adja meg.
1. Mátrix tesztelése:

**DRM**|**Böngészőben**|**Eredmény jogosult felhasználók**|**Eredményt nem jogosult felhasználók**
---|---|-----|---------
**PlayReady**|MS él vagy IE11 a Windows 10 rendszerben|A sikeres|FAIL:
**Widevine**|A Windows 10 rendszerben a Chrome-ban|A sikeres|FAIL:
**FairPlay** |TBD||

Az Azure Media Services csapata György Trifonov van írt kezeléséről a részletes lépéseket az Azure Active Directory ASP.NET MVC player alkalmazás beállítását blog: [Azure Media Services OWIN MVC integráció alapú alkalmazást és az Azure Active Directory és JWT jogcímeken alapuló kulcs tartalomkézbesítési korlátozása](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

György is magában írt blog [JWT jogkivonat hitelesítési az Azure Media Services és dinamikus titkosítást](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). Pedig a [minta a Azure Active Directory funkciók integrálása az Azure Media Services fő kézbesítési](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Azure Active Directory olvashat:

- Fejlesztőeszközök információt találhat az [Azure Active Directory fejlesztői útmutató](../active-directory/active-directory-developers-guide.md).
- Információk rendszergazdáknak a [Felügyelete Your Azure Active Directory](../active-directory/active-directory-administer.md)található.

### <a name="some-gotchas-in-implementation"></a>Néhány meglátások végrehajtása

Vannak bizonyos "meglátások" végrehajtása. Remélhetőleg az alábbi listában szereplő "meglátások" segíthetnek abban az esetben, ha hiba lép fel hibaelhárítás.

1. **Kibocsátó** URL-címe **"/"**kell végződnie.  

    **Célközönség** kell a lejátszó ügyfél-azonosító, és azt is fel kell **"/"** végén található a kibocsátó URL-CÍMÉT.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    Az [JWT Médiadekódoló](http://jwt.calebb.net/)meg kell jelennie **és** és **iss** mint alább a JWT jogkivonat:
    
    ![gotcha 1.](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Engedélyek hozzáadása az alkalmazás AAD (a beállítás lapon az alkalmazás). Ez a szükséges az egyes alkalmazások (helyi és telepített verziója).

    ![gotcha 2.](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. A dinamikus CENC védelem beállítása a megfelelő kibocsátó használja:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    A következő nem működik:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    A globálisan egyedi azonosítója a AAD bérlői azonosítóját. A globálisan egyedi azonosítója végpontok előugró találhatók az Azure-portálon.

4. Engedélyek biztosítása csoporttagság jogosultságokkal követelések. Ellenőrizze, hogy az AAD alkalmazás nyilvánvalóan fájlt, a következő felkínálunk

    "groupMembershipClaims": "Az összes", (az alapértelmezett értéke null)

5. A beállítás megfelelő TokenType korlátozás követelmények létrehozásakor.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Támogatás a JWT (AAD) mellett SWT (ACS) ad, mivel TokenType alapértelmezés szerint TokenType.JWT. Ha SWT/ACS használ, be kell TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>További témakörök végrehajtása
Ezután azt fogja telepítőlemezének uss néhány további témakörök a tervezéséhez és kivitelezéséhez.

###<a name="http-or-https"></a>Http- vagy HTTPS?

A következő támogatnia kell az ASP.NET MVC player alkalmazás úgy alakítottuk ki:

1. Azure Active Directory, a HTTPS; kell keresztül a felhasználói hitelesítés
1. JWT jogkivonat exchange-ügyfél és Azure Active Directory, a HTTPS; kell között
1. DRM licenc WIA az ügyfél, amely lehet a HTTPS, ha az Azure Media Services által biztosított licenc kézbesítési szükséges. Természetesen PlayReady termékcsomag határozza meg, hogy HTTPS-licenc kézbesítési. Ha a PlayReady licenc server Azure Media Services kívül esik, felhasználható HTTP vagy HTTPS.

Az ASP.NET-player alkalmazásban emiatt használható a HTTPS legjobb módszer. Ez azt jelenti, hogy az Azure Media Player egy oldalra a HTTPS lesz. Azonban a folyamatos átvitelű azt inkább HTTP, ezért érdemes vegyes tartalom probléma szükség.

1. Böngésző vegyes tartalom nem teszi lehetővé. De például zökkenőmentes OSMF és a Silverlight bővítményt, és a SZAGGATÁS bővítmények engedélyezése. Vegyes tartalom egy biztonsági problémát – ennek oka az, hogy az azt jelenti, hogy rosszindulatú JS, amelyek a felhasználói adatok kockázatára okozhatják behelyezése veszélyével.  Böngészők blokkolásához alapértelmezés szerint és az eddigi csak úgy lehet kerülheti meg a kiszolgáló (origin) oldalon engedélyezése minden tartományban (függetlenül attól, hogy https vagy http). Ez az valószínűleg érvénytelen célszerű vagy.
1. Azt lehetőleg ne vegyes tartalom: akár mindkettőt használja a HTTP vagy mindkettőt HTTPS. Vegyes tartalom lejátszásakor silverlightSS technikai szükséges, törölje a jelet a vegyes tartalom figyelmeztetés. flashSS technikai vegyes tartalom figyelmeztetés nélkül vegyes tartalom kezeli.
1. Ha a továbbított végpont előtt augusztus 2014-es jött létre, akkor HTTPS nem támogatja. Ebben az esetben kérjük, létrehozása és használata az új adatfolyam végpont HTTPS.

Hivatkozás végrehajtására DRM védett tartalmat, az alkalmazás és a folyamatos átvitelű területen kell keresnie HTTTPS. Nyissa meg a tartalom a Windows Media player nem szükséges hitelesítési vagy licenccel, így Önnek nem kell a HTTP vagy a HTTPS szabadság.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory fő átváltási aláírása

Ez a fontos mutasson a végrehajtás veszi figyelembe. Nem tekinti meg a végrehajtás, ha a bejegyzett rendszer ahányat leállítása munka teljesen belül legfeljebb 6 hét.

Azure Active Directory iparági szabvány használja megbízhatóvá magát és Azure Active Directory használó alkalmazásai között. Azure Active Directory kifejezetten, használja a egy aláírási kulcs, amely a nyilvános és titkos kulcs két áll. Amikor Azure Active Directory létrehozza a biztonsági jogkivonat, amely tartalmazza a felhasználó adatai, akkor a token Azure Active Directory, a titkos kulccsal vissza az alkalmazást az elküldés előtt írja alá. Ellenőrizze, hogy a token érvényes és Azure Active Directory a ténylegesen kezdeményezésű, az alkalmazás ellenőriznie kell a nyilvános kulccsal Azure Active Directory összevonási metaadat-dokumentum a bérlő lévő által elérhetővé tett a token aláírás. A nyilvános kulcshoz – és az aláírási kulcs, amelyből származik – megegyezik egy használt összes bérlők az Azure Active Directory.

Részletes információkat a fő Azure AD-átváltási található a dokumentumban: [Bejelentkezés kulcs átváltási az Azure Active Directory fontos információkat](../active-directory/active-directory-signing-key-rollover.md).

A [nyilvános-titkos kulcs pár](https://login.windows.net/common/discovery/keys/)között 

- A titkos kulcs van Azure Active Directory létrehozásához használja a JWT jogkivonat;
- A nyilvános kulcs segítségével egy alkalmazást, például a DRM licenc kézbesítési szolgáltatások AMS ellenőrizze a JWT jogkivonat;
 
Biztonsági célra az Azure Active Directory elforgatása a tanúsítvány rendszeres időközönként (6 hét). Abban az esetben, ha biztonsági szabályok megsértésével a fő átváltási akkor fordulhat elő, bármikor. Ezért AMS licenc kézbesítési szolgáltatások frissítenie kell a nyilvános kulcs Azure Active Directory forgatása a fontosabb pár, egyéb esetben AMS jogkivonat hitelesítés sikertelen lesz, és nincsenek licencek indítandó használják. 

Ez TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument megadásával DRM licenc kézbesítési szolgáltatások konfigurálásakor érhető el.

A JWT jogkivonat folyamat van, mint alább:

1.  Azure Active Directory adnak a JWT jogkivonat egy hitelesített felhasználó; aktuális titkos kulccsal
2.  Amikor egy lát egy CENC a többszörös DRM védett tartalmat, azt először keresse meg az Azure Active Directory által kibocsátott JWT jogkivonathoz.
3.  A Windows Media player küld licenc beszerzése a JWT jogkivonat a licenc kézbesítési szolgáltatások AMS;
4.  A licenc kézbesítési szolgáltatások AMS Azure AD az aktuális érvényes nyilvános kulcs használatával ellenőrizze a JWT jogkivonat kibocsátása licencek előtt.

DRM licenc kézbesítési szolgáltatások mindig keresi az Azure Active Directory aktuális érvényes nyilvános kulcs. A nyilvános kulcs Azure Active Directory által bemutatott lesz az Azure Active Directory által kibocsátott JWT token ellenőrzéséhez használt kulcsot.

Mi történik, ha a fő átváltási AAD létrehoz egy JWT jogkivonat, de a JWT előtt token van által küldött játékosokat DRM licenc kézbesítési szolgáltatások AMS a tartomány ellenőrzéséhez történik? 

Előfordulhat, hogy egy kulcsot bármikor vetítve, mert nincs mindig egynél több érvényes nyilvános kulcs az összevonási metaadatok dokumentumokban használható. Azure Media Services licenc kézbesítési használhatja a billentyűket, a dokumentumot, a megadott egy kulcsot hamarosan vetítve is, mivel egy másik előfordulhat, hogy a helyettesítő, és így tovább.

### <a name="where-is-the-access-token"></a>Hol található a hozzáférési jogkivonat?

Ha megnézi hogyan a webalkalmazást felhívja a [OAuth 2.0 ügyfél hitelesítő adatok megadása az alkalmazás azonosítója](active-directory-authentication-scenarios.md#web-application-to-web-api)API alkalmazás, a hitelesítési folyamat megegyezik alatt:

1.  A felhasználó be van jelentkezve a webalkalmazás az Azure Active Directory (lásd: a [Webböngészőben webalkalmazás](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Az Azure Active Directory engedélyezési végpontot átirányítja a felhasználói ügynököt a ügyfélalkalmazás engedélyezési kóddal. A felhasználói ügynököt a ügyfélalkalmazás átirányítási URI engedélyezési kód adja vissza.
3.  A webes alkalmazás kell szerezheti be egy hozzáférési jogkivonat, így hitelesíteni a webes API-val és a kívánt erőforrás beolvasásához. Azure Active Directory jogkivonat végpontra a hitelesítő adatokat, az ügyfél-azonosító és a webes API-alkalmazás azonosítója URI van egy kérelmet. Jelenítse meg a engedélyezési kódot, amellyel igazolhatja, hogy a felhasználó hozzájárult.
4.  Azure AD az alkalmazás hitelesíti, és egy JWT hozzáférési jogkivonat, hívja fel a webes API-val használt adja eredményül.
5.  HTTPS a webes alkalmazás segítségével a visszaadott JWT jogkivonat a kérelem engedélyezési fejlécében a JWT karakterlánc, egy "Bearer" megnevezéssel hozzáadása a webes API-val. A webes API majd ellenőrzi a JWT jogkivonat, és ha érvényesítése sikeres, ad eredményül a kívánt erőforrás.

A "alkalmazás azonosítója" forgalom a webes API megbízik, hogy a webalkalmazás hitelesített felhasználó. Emiatt a minta egy megbízható alrendszer neve. A [diagram ezen az oldalon](http://msdn.microsoft.com/library/azure/dn645542.aspx/) ismerteti, hogyan engedélyezési kód adja meg az adatfolyam működik.

Licenc beszerzési jogkivonat korlátozás azt az azonos megbízható alrendszer mintát követik. És a kézbesítési szolgáltatás az Azure Media Services a webes API erőforrás, eléréséhez szükséges webalkalmazás "kódmentes erőforrás". Hogy hol található a hozzáférési jogkivonat?

Sor kerülhet hogy vannak lekérését hozzáférési jogkivonat Azure AD. A felhasználó sikeres hitelesítés után engedélyezési kód adja vissza. A engedélyezési kód ezután használja, ügyfél azonosítója és az alkalmazás billentyűt, és az exchange-hozzáférési jogkivonat. És a hozzáférési jogkivonat "mutató" alkalmazás mutató elérése vagy az Azure Media Services licenc kézbesítési szolgáltatás, amely.

Szükséges regisztrálhat, és állítsa be a "mutató" alkalmazást az Azure Active Directory az alábbi lépéseket követve:

1.  Az Azure Active Directory-ös bérlői

    - bejelentkezés az URL-címmel alkalmazás (erőforrások) hozzáadása: 

    https://[resource_name].azurewebsites.NET/ és 

    - az alkalmazás azonosítója URL-címe: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Az erőforrás-alkalmazásokban; új kulcs hozzáadása
3.  Frissítse a nyilvánvalóan app-fájl, hogy a groupMembershipClaims tulajdonság a következő értéket tartalmazza: "groupMembershipClaims": "Az összes",  
4.  A alkalmazásban Azure AD a lejátszó web app mutató szakaszában "egyéb alkalmazások engedélyek", adja meg az erőforrás hozzá lett adva a fenti 1 alkalmazást. "Meghatalmazott engedélyeit" csoportban jelölje be a "Hozzáférés [resource_name]" be van jelölve. Ezzel a módszerrel a web app-engedély az erőforrás-alkalmazás megnyitása az access tokenek létrehozásához. Meg kell ehhez a web app helyi és a telepíthető változata Ha Visual Studio és Azure web app alkalmazással.
    
Emiatt az Azure Active Directory által kibocsátott JWT jogkivonathoz valóban az jogkivonat "mutató" erőforrás eléréséhez.

### <a name="what-about-live-streaming"></a>Mit kell tudni élő adatfolyam?

A fenti a vitafórum van már összpontosul igény szerinti eszközök. Mit kell tudni live streaming?

A jó hír az, hogy-et is pontosan a azonos tervezés és a végrehajtásához élő streaming az Azure Media Services való az eszköz "VOD eszközként" program társított kezelésével védelme.

Kifejezetten célszerű a jól ismert, hogy a teendő az Azure Media Services streaming élő létrehozásához szükséges egy csatornát, majd a csatorna a program. Szeretne létrehozni, a program, amely tartalmazza a program az élő archívum tárgyi eszköz létrehozásához szükséges. Annak érdekében, hogy CENC élő tartalom többszörös-DRM védelemmel, Önnek kell elvégeznie, akkor alkalmazása ugyanabban a telepítő/feldolgozása az eszközre, mintha egy "VOD eszköz" a program indítása előtt volt.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Mit kell tudni az Azure Media Services kívüli licenc kiszolgálók?

Gyakran ügyfelek előfordulhat, hogy rendelkezik fekteti licenc kiszolgálófarm saját adatok középre vagy üzemeltetett DRM szolgáltatók. Szerencsére Azure Media Services tartalomvédelem lehetővé teszi, hogy a hibrid módban: tartalmát is, és közben DRM licencek érkeznek kívül az Azure Media Services-kiszolgálók az Azure Media Services dinamikusan védett. Ebben az esetben a következő szempontokat mérlegelve végrehajtott módosítások létezik:

1. A biztonságos jogkivonat szolgáltatás kell tokenek, amelyek elfogadható, és ellenőrizheti a licenc kiszolgálófarm hiba. A Widevine licenc kiszolgálók Axinom által biztosított például egy adott JWT jogkivonat, amely tartalmazza az "üzenet jogosultság" van szükség. Ezért van szükség egy ilyen JWT jogkivonat kibocsátása STS. A szerzők befejeződött egy végrehajtás és olvashat az [Azure dokumentáció](https://azure.microsoft.com/documentation/)központban az alábbi dokumentum: [Segítségével Axinom előadásához az Azure Media Services Widevine licenceket](media-services-axinom-integration.md). 
1. Már nincs szüksége az Azure Media Services konfigurálása a licenc kézbesítési szolgáltatás (ContentKeyAuthorizationPolicy). Mit kell tennie azt javasoljuk, hogy a szükséges licenc WIA URL-ek (PlayReady, Widevine és FairPlay) Ha AssetDeliveryPolicy konfigurálása a többszörös-DRM CENC beállítani.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Mi történik, ha használni kívánt egy egyéni STS?

Ennek oka lehet, néhány, egy ügyfél előfordulhat, hogy egy egyéni STS (jogkivonat szolgáltatás biztonságos) használata JWT tokenek nyújtó választva. Ezek a következők:

1.  Az identitás szolgáltató (IDP) ügyfél által használt STS nem támogatja. Ebben az esetben a egy egyéni STS lehet egy lehetőséget.
2.  Az ügyfél rugalmasabbá és szorosabb vezérlő az ügyfél előfizetői rendszer számlázási STS integrálása szükség. Ha például MVPD operátor ajánlhat fel több OTT előfizetői csomagok például egyszerű prémium verzióban, a sports stb. Az operátor érdemes egy jogkivonat előfizető csomaggal követelések megfelelően az, hogy csak a megfelelő csomag tartalma elérhetővé válik. Ebben az esetben egy egyéni STS a szükséges rugalmasság és a vezérlő biztosít.

Két módosítani szeretne tenni egy egyéni STS használata esetén:

1.  Tárgyi eszköz licenc kézbesítési szolgáltatás konfigurálásakor kell adja meg a biztonsági kulcsot igazolás Azure Active Directoryból az aktuális kulcs helyett egyéni STS (További részletek az alábbi) által használt.
2.  Egy JTW token létrehozásakor biztonsági kulcs helyett az aktuális X509, a titkos kulcs van-e megadva az Azure Active Directory tanúsítvány.

Biztonsági billentyűparancsok két típusa van:

1.  Szimmetrikus kulcs: ugyanazt a kulcsot használatos létrehozása és a JWT jogkivonat; ellenőrzése
2.  Aszimmetrikus kulcs: egy X509 tanúsítvány és a titkos kulcs titkosítja/JWT jogkivonat és létrehozásához a nyilvános kulcs a token ellenőrzéséhez használja a két nyilvános kulcs.

####<a name="tech-note"></a>Technikai Megjegyzés

.NET-keretrendszer használatakor és C#, a fejlesztői platform, a X509 aszimmetrikus biztonsági kulcs használt tanúsítvány kulcs hossza legalább 2048 kell rendelkeznie. Ez az a osztály a .NET-keretrendszer System.IdentityModel.Tokens.X509AsymmetricSecurityKey követelmény. Egyéb esetben a következő kivételt fog elő:

IDX10630: Az aláírás "System.IdentityModel.Tokens.X509AsymmetricSecurityKey" nem lehet kisebb, mint "2048" bittel. 

## <a name="the-completed-system-and-test"></a>A kész rendszer és tesztelése

Fog végigvezetjük néhány esetek a bejegyzett végpont rendszerben, hogy az olvasóknak beállíthatja, hogy egy basic "kép" viselkedésének előtt bejelentkezési fiókot.

A lejátszó webalkalmazás és a bejelentkezési megtalálható [Itt](https://openidconnectweb.azurewebsites.net/).

Ha szükségesek "nem integrált" forgatókönyv: az Azure Media Services vannak vagy nem védett, illetve DRM védett, de jogkivonat hitelesítés nélkül is videó eszközök (licenc kiadását személy, aki azt kérő), tesztelheti, bejelentkezés nélkül (által átirányítása HTTP, ha a videó a folyamatos átvitelű HTTP).

Ha a keresett végpontok közötti integrált forgatókönyv: videó eszközök jogkivonat hitelesítéssel és Azure Active Directory által készített JWT jogkivonat az Azure Media Services dinamikus DRM védelem alatt áll, be kell jelentkeznie.

### <a name="user-login"></a>Felhasználói bejelentkezés

Annak érdekében, hogy a végpontok közötti integrált DRM rendszer teszteléséhez "fiók" vagy a hozzáadott van szükség. 

Milyen fiókot?

Bár a Azure eredetileg engedélyezett csak a Microsoft-fiók felhasználók hozzáférésének, most mindkét rendszerekből felhasználók lehetővé teszi. Ez végezték úgy, hogy az összes az Azure tulajdonságok adatvédelmi Azure AD-hitelesítést végezni a szervezeti felhasználók, amelynek hitelesítéshez Azure Active Directory és összevonási kapcsolat létrehozásával és Azure Active Directory hol bizalmi kapcsolatok a Microsoft-fogyasztói identitás rendszer fogyasztói felhasználók hitelesítést végezni. Azure AD eredményt adja, tudni hitelesítést végezni a Microsoft-fiókok "Vendég", valamint a "eredeti" Azure Active Directory-fiókok.

Mivel az Azure Active Directory és a Microsoft-fiók (MSA) tartomány bizalmi kapcsolatok, felveheti az egyéni Azure ad a következő tartományok bármely fiókok bérlői, és jelentkezzen be a fiók használata:

**Tartománynév**|**Tartomány**
-----------|----------
**Egyéni Azure AD-bérlő tartomány**|somename.onmicrosoft.com
**Vállalati tartomány**|Microsoft.com
**A Microsoft-fiók (MSA) tartomány**|Outlook.com, live.com, Hotmail.com-os

A szerzők van egy fiókom létrehozott vagy a hozzáadott, előfordulhat, hogy forduljon. 

Alul a képernyőképek más tartományi fiókok által használt különböző bejelentkezési lapok láthatók.

**Egyéni Azure AD-bérlő tartományfiók**: Ebben az esetben, oldal jelenik meg a testre szabott bejelentkezési egyéni Azure AD-bérlő tartományt.

![Egyéni Azure AD-bérlő tartományi fiók](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**A Microsoft tartományi fiók intelligens kártyával**: Ebben az esetben megjelenik a Microsoft vállalati egyéni bejelentkezési lap informatikai kétfaktoros hitelesítés.

![Egyéni Azure AD-bérlő tartományi fiók](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**A Microsoft-fiók (MSA)**: Ebben az esetben jelennek meg a Microsoft-Account bejelentkezési lapja vonzóbbak lehetnek.

![Egyéni Azure AD-bérlő tartományi fiók](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Titkosított Media bővítmények használata PlayReady

A titkosított Media bővítmények (EME) rétegen a modern böngészőben PlayReady ügyfélszolgálatával, például a Windows 8.1 és a másolatot Internet Explorer 11-es és a Windows 10, a Microsoft Edge böngészővel PlayReady lesz a mögöttes DRM EME az.

![PlayReady EME verzióval](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

A sötét player terület van, annak oka, hogy PlayReady védelem megakadályozza, hogy az egyik abban, hogy a védett videó képernyőkép rögzítése. 

A következő képen látható a lejátszó bővítmények MSE/EME támogatás.

![PlayReady EME verzióval](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

A Microsoft Edge és a Windows 10 Internet Explorer 11-es EME lehetővé teszi, hogy a Windows 10-eszközök, amelyek támogatják a [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) meghívása. PlayReady SL3000 feloldása a folyamat a bővített funkciójú kiemelt tartalmak (4K, HDR, stb.) és az új tartalomkézbesítési modell (korai ablak bővített tartalmak esetében).

Koncentráljon a Windows-eszközökön: PlayReady csak a DRM, a Windows-eszközön (PlayReady SL3000) elérhető a Hardveres. Adatfolyam szolgáltatás használhatja PlayReady EME vagy egy UWP alkalmazás keresztül, és egy újabb videó minőségét PlayReady SL3000, mint a másik DRM felajánl. 2K tartalmát általában a Chrome vagy a Firefox és a 4K tartalmát Microsoft Edge/IE11 vagy egy UWP alkalmazás ugyanarra az eszközre (attól függően, hogy szolgáltatásbeállításokkal és a végrehajtás) keresztül fog flow.


#### <a name="using-eme-for-widevine"></a>Widevine EME verzióval

Google Widevine EME/Widevine támogatás, például a Chrome 41 + a Windows 10, a Windows 8.1, a Mac OSX Yosemite és a Chrome az Android 4.4.4, modern böngésző a DRM EME mögött.

![Widevine EME verzióval](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Figyelje meg, hogy Widevine szintén nem akadályozza meg, a képernyőfelvétel ne egy videó védett.

![Widevine EME verzióval](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Nem jogosult felhasználók

Ha egy felhasználó nem "Jogosult felhasználók" csoport tagjának, a felhasználó nem tud "jogosultság jelölőnégyzet" megfelelt, és a többszörös-DRM licenc szolgáltatást fogja megtagadja a kért licenc kiadását az alább látható módon. A részletes leírás "licenc szerezheti be nem sikerült", amely olyan, mint megtervezni.

![Nem jogosult felhasználók](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Egyéni biztonságos jogkivonat szolgáltatást futtató

Egyéni biztonságos jogkivonat szolgáltatás (STS) futó példánkban a JWT jogkivonat által indítandó az egyéni STS szimmetrikus vagy aszimmetrikus billentyűje segítségével. 

A kis-és szimmetrikus kulccsal (Chrome használata):

![Egyéni STS fut](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

A kis-és egy X509 keresztül aszimmetrikus kulccsal certificate (Microsoft modern böngésző használatával).

![Egyéni STS fut](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

A fenti esetek egyike, a felhasználói hitelesítés munkamennyiségének – Azure Active Directory keresztül. Az egyetlen különbség, hogy JWT tokenek az Azure Active Directory helyett egyéni STS vannak-e ki. Természetesen dinamikus CENC védelmet konfigurálásakor korlátozása licenc kézbesítési szolgáltatás, adja meg a JWT jogkivonat, szimmetrikus vagy aszimmetrikus billentyűt.

## <a name="summary"></a>Összefoglalás

A dokumentumban, hogy CENC egyeztetni keresztül jogkivonat hitelesítési többfázisú native DRM és a hozzáférés-vezérlés: a tervezés és Azure, az Azure Media Services és az Azure Media Player végrehajtása.

- Egy hivatkozás tervezés látható, amely tartalmazza az összes szükséges összetevőt DRM/CENC alrendszer;
- A hivatkozás megvalósítás Azure, az Azure Media Services és az Azure Media Player.
- Néhány közvetlenül a tervezéséhez és kivitelezéséhez foglalkozó témaköröket is tartalmazza.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Visszaigazolások 

William Zhang Mingfei Pozsony, Dániel Le frankot számít, Kilroy Zoltán, Ágnes Kornich
