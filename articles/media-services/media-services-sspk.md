<properties 
    pageTitle="Licencelési Microsoft® zökkenőmentes adatfolyam ügyfél portolása Kit" 
    description="További tudnivalók a Microsoft® zökkenőmentes Streaming ügyfél portolása Kit licencelési." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licencelési Microsoft® zökkenőmentes adatfolyam ügyfél portolása Kit

##<a name="overview"></a>– Áttekintés

Microsoft zökkenőmentes Streaming ügyfél portolása Kit (**SSPK** rövid) és a folyamatos zökkenőmentes ügyfél megvalósítás beágyazott eszközgyártók, kábel és mobilszolgáltatók, tartalom szolgáltatók, kézibeszélőt gyártók, független szoftver keresve, és megoldást szolgáltatók termékek és szolgáltatások folyamatos zökkenőmentes Streaming format adaptív adatfolyam tartalmának kialakításához segítséget optimalizált. SSPK egy eszköz és platform független végrehajtásának zökkenőmentes Streaming ügyfél, amely a licenciavevő bármely eszközön, és a platform által átültetésre is lehet. 

Alatt helyezi el a magas szintű architektúrájának és IIS zökkenőmentes Streaming portolása Kit mezőben folyamatos zökkenőmentes ügyfél megvalósítása a Microsoft által nyújtott és tartalom zökkenőmentes Streaming lejátszásra core logika tartalmazza. Ez akkor majd átültetésre eszközre vagy platform-partnerek által megfelelő felületek alkalmazásával. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Leírás

A kiváló üzleti értéket tartalmazó kifejezések SSPK licencbe kapta. SSPK licenc, az üzleti nyújtja:

- Zökkenőmentes portolása Kit Streaming forrás c++ 
  - Zökkenőmentes Streaming ügyfél funkciókat megvalósítása
  - összeadja a formátum elemzése, heurisztikát, pufferelési logika stb.
- Lejátszó alkalmazást API-hoz 
  - programozási felületek való interakció egy media player alkalmazásban
- Platform hardverabsztrakciós réteg (PAL) felület 
  - az operációs rendszer (szálak, sockets) interakció alkalmazásprogramozási felületek
- Hardveres hardverabsztrakciós réteg (HAL) felület 
  - A hardverrel való interakciót felületek programozási / V dekóderek (dekódolását, a megjeleníthető)
- Digitális jogkezelési kezelőfelület (DRM) 
  - programozási felületek DRM keresztül DRM hardverabsztrakciós réteg (DAL) kezelése
  - A Microsoft PlayReady portolása Kit szállított külön-külön, de integrálja a kapcsolaton keresztül. További információt a Microsoft PlayReady Device licencelése, kattintson [ide](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- A minták végrehajtása 
  - Példa a Linux PAL végrehajtása
  - minta HAL végrehajtására GStreamer

##<a name="licensing-options"></a>Licencelési lehetőségekről

Microsoft zökkenőmentes Streaming ügyfél portolása Kit számára hozzáférhető licenciavevők két különböző licencszerződést alatt: egy zökkenőmentes Streaming ügyfél ideiglenes termékek és egy másik zökkenőmentes Streaming ügyfél végtermék terjesztésével végfelhasználóknak fejlesztésével.
 
- A lapkakészlet gyártók, a rendszer kiegészítők vagy a független szoftver keresve ideiglenes termékek fejlesztésére kit portolása forráskód igénylő **Ideiglenes termék licenc** a Microsoft zökkenőmentes Streaming ügyfél portolása Kit kell végrehajtani.
- Eszközgyártók vagy igénylő terjesztési tartalomvédelmi zökkenőmentes Streaming ügyfél végleges termékek végfelhasználói ISV a Microsoft zökkenőmentes Streaming ügyfél portolása Kit **Végleges termék licencet** kell végrehajtani.

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>A folyamatos átvitelű Kit ideiglenes termék licencét portolása ügyfél Microsoft sima

A licenc, csoportban Microsoft felajánlja a különféle a zökkenőmentes Streaming ügyfél portolása Kit és a szükséges szerződés KIZÁRÓLAGOSSÁGA fejlesztése, és zavartalan Streaming ügyfél ideiglenes termékek juttatnia más zökkenőmentes Streaming ügyfél portolása Kit eszköz licenciavevők, amely zökkenőmentes Streaming ügyfél végtermék terjesztése.

####<a name="fee-structure"></a>Díj struktúra

Amerikai Egyesült Államok 50 000 Ft egyszeri licenc díjat hozzáférést biztosít a zökkenőmentes Streaming ügyfél portolása Kit. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>A folyamatos átvitelű Kit véglegesként termék licencét portolása ügyfél Microsoft sima

Ez a licenc a Microsoft kínál minden szükséges szerződés KIZÁRÓLAGOSSÁGA zökkenőmentes Streaming ügyfél ideiglenes termékek kapott más zökkenőmentes Streaming ügyfél portolása Kit licenciavevők és a vállalat márka zökkenőmentes Streaming ügyfél végtermék végfelhasználói terjesztése.

####<a name="fee-structure"></a>Díj struktúra

A zökkenőmentes Streaming ügyfél végtermék csoportban, a kiemelt modell kínálják:

- $0,10 per teljesült eszköz végrehajtása
- A kiemelt van felső határa 50 000 Ft minden évben
- Az első 10 000 eszköz megvalósítás évente nem kiemelt 

##<a name="licensing-procedure-and-sspk-access"></a>Licencelési eljárás és SSPK elérése

Kérjük, e-mailben [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) összes licencelési lekérdezések.

Az [eloszlás SSPK portál](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) való regisztrált ideiglenes licenciavevők érhető el.

Ideiglenes és végleges SSPK licenciavevők elküldhetik technikai kérdések [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>A folyamatos átvitelű ügyfél köztes termék szerződés licenciavevők Microsoft sima

- Adroit üzleti megoldások, Inc.
- Speciális digitális adás Nyelvű
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Alticast Corporation
- Amazon digitális szolgáltatások Iparcikk
- AVC multimédia szoftver, Reinhard
- Cavium Iparcikk
- EchoStar vásárlásával Corporation
- Enseo Iparcikk
- Fluendo S.A.
- HANDAN BroadInfoCom, Reinhard
- Infomir GMBH
- Irdeto USA Inc.
- Szabadság globális szolgáltatások BV
- MediaTek Inc.
- További MStar Ltd.
- Nintendo, Reinhard
- OpenTV Iparcikk
- Sáfrány digitális korlátozva van
- Szecsuan Changhong elektromos Co. Ltd.
- SoftAtHome
- Sony Corporation
- Tatung Technology Inc.
- TCL Technoly elektronikai (Huizhou), Reinhard
- Ticaret A.S. Vestel Elektronik Sanayi helyezés
- VisualOn Iparcikk
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>A folyamatos átvitelű ügyfél végtermék szerződés licenciavevők Microsoft sima

- Speciális digitális adás Nyelvű
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Amazon digitális szolgáltatások Iparcikk
- AmTRAN technológia, Reinhard
- Arcadyan technológia Corporation
- ATMACA ELEKTRONİK SAN. MENTÉS TİC. A.Ş
- A brit égbolt közvetítése korlátozva van
- CastPal Technology Inc., Shenzhen
- Compal elektronikai Iparcikk
- Dongguan digitális AV technológia Corp. Ltd.
- EchoStar vásárlásával Corporation
- Enseo Iparcikk
- Filmflex filmek korlátozva van
- Fluendo S.A.
- Gibson újítás korlátozva van
- Haier információk Applicantion S.R.L
- HANDAN BroadInfoCom, Reinhard
- Homecast Co. Ltd.
- HON Hai pontosság ipari, Reinhard
- Infomir GMBH
- Kaonmedia, Reinhard
- KDDI Corporation
- Nintendo, Reinhard
- Narancssárga Nyelvű
- Sáfrány digitális korlátozva van
- Sagemcom szélessávú Társítások
- Shenzhen Coship elektronikai CO. Ltd.
- Shenzhen Jiuzhou elektromos Co. Ltd.
- Shenzhen Skyworth digitális technológia Co. Ltd.
- Szecsuan Changhong elektromos, Reinhard
- Skardin ipari Corp.
- Ég Deutschland Fernsehen GmbH & Co. KG
- SmarDTV S.A.
- SoftAtHome
- Sony Corporation
- Korlátozott TCL tengerentúli Marketing (tengeri Makaó kereskedelmi célú)
- Technicolor kézbesítési technológiák Társítások
- Tongfang globális Ltd.
- Toshiba életmód-termékeket és szolgáltatásokat Corporation
- Univerzális Media Corporation /Slovakia/ s.r.o.
- VIZIO Iparcikk
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
