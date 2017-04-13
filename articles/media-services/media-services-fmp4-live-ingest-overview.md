<properties 
    pageTitle="Azure Media Services töredezett MP4 live ingest specifikációja |} Microsoft Azure" 
    description="Ez a meghatározás Protocol (protokoll) és a Microsoft Azure Media Services adatfolyam szempontjából élő alapú tördelt MP4 formátumban ismerteti. Microsoft Azure Media Services élő adatfolyam szolgáltatás, amely lehetővé teszi a felhasználóknak adatfolyam-élő események és a valós idejű tartalmának közvetítése biztosít a Microsoft Azure a felhőben platform használják. A dokumentum is azt ismerteti, hogy a legjobb tanácsok az élő épület erősen felesleges és robusztus ingest mechanizmusok." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services töredezett MP4 live ingest meghatározása

Ez a meghatározás Protocol (protokoll) és a Microsoft Azure Media Services adatfolyam szempontjából élő alapú tördelt MP4 formátumban ismerteti. Microsoft Azure Media Services élő adatfolyam szolgáltatás, amely lehetővé teszi a felhasználóknak adatfolyam-élő események és a valós idejű tartalmának közvetítése biztosít a Microsoft Azure a felhőben platform használják. A dokumentum is azt ismerteti, hogy a legjobb tanácsok az élő épület erősen felesleges és robusztus ingest mechanizmusok.


##<a name="1-conformance-notation"></a>1. a megfelelőség jelölés

Oklevéltípus "kell", "Nem", "Szükséges", "SHALL", "Nem kell", "SHOULD", "Nem CÉLSZERŰ", "Ajánlott", "Előfordulhat, hogy" és "nem" kötelező, a dokumentum nem értelmezhetők RFC 2119 ismertetett módon.

##<a name="2-service-diagram"></a>2. a szolgáltatás Diagram 

Az alábbi ábrán a magas szintű architektúrájának az élő adatfolyam szolgáltatás a Microsoft Azure Media Services jeleníti meg:

1.  Élő Encoder verembe küldi, a Microsoft Azure Media Services SDK keresztül élő hírcsatornák csatornák, amelyeket létrehozott, és kiépítéstől be.
2.  Csatornák, a programok és a folyamatos átvitelű a Microsoft Azure Media Services fogópont végpont ingest, formázás, beleértve az összes élő adatfolyam funkciók a felhő DVR, biztonsági, méretezhetőség és redundancia.
3.  Ügyfelek sikerült kérheti üzembe CDN réteg az adatfolyam-végpontot, és az ügyfél végpontok között.
4.  Ügyfél végpontok adatfolyam a az adatfolyam-végpontot (pl. zökkenőmentes adatfolyam-, vonal, HDS vagy HLS) HTTP adaptív Streaming protokollok használatával.

![image1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. bit-adatfolyam-formátum – ISO 14496 12 tördelt MP4

A egybeírt az élő streaming ingest dokumentumtartalom tárgyalt [ISO-14496 – 12] alapul. Olvassa el az [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) részletes magyarázatot tördelt MP4 formátumban és bővítmények mindkét igény szerinti videó fájlokat, és a folyamatos átvitelű bevitel live.

###<a name="live-ingest-format-definitions"></a>Élő ingest formátum definíciók 

Az alábbi figyel az élő vonatkozó definíciók ingest be a Microsoft Azure Media Services speciális formátumban:

1. A "ftyp", a LiveServerManifestBox és a "moov" párbeszédpanel minden kérésével (HTTP-bejegyzés) kell elküldeni.  El kell küldenie a az adatfolyam elejére, és a kódoló kell csatlakoznia önéletrajz bármikor adatfolyam ingest.  Olvassa el a [1] rész 6 további információt.
2. A szakasz 3.3.2 [1] határozza meg egy nem kötelező mezőben StreamManifestBox az élő nevű ingest. A Microsoft Azure terheléselosztó útválasztási logika, miatt ebbe a mezőbe használatát elavult, és meg nem jelenik meg, ha a Microsoft Azure Media üzembe ingesting. Ez a mező nem tartalmaz adatokat, Azure Media Services meg automatikusan figyelmen kívül hagyja.
3. A [1] 3.2.3.2 meghatározott TrackFragmentExtendedHeaderBox minden részlet jelen kell lennie.
4. Médiafájlok szegmensek azonos URL-címekkel készítése a több adatközpont esetén 2-es verziója a TrackFragmentExtendedHeaderBox kell használni. A részlet index mező szükséges határokon-adatközponthoz feladatátvételét streaming tárgymutató-alapú formátumok, például az Apple HTTP Live Streaming (HLS) és a tárgymutató-alapú MPEG-vonal.  Ahhoz, hogy az idegen-adatközponthoz feladatátvevő, a fragment index szinkronizálni kell több kódolók és növelése az egyes egymást követő media fragment 1-gyel még több kódoló újraindítása és a hibák.
5. [1] 3.3.6 szakaszában a mező neve (End-a-adatfolyam) EOS jelezheti a csatorna küldhető élő bevitel végén MovieFragmentRandomAccessBox (mfra) határozza meg. Az Azure Media Services ingest logika, miatt EOS (End-a-adatfolyam) használatát elavult, és a "mfra" mezőben élő szempontjából nem kell elküldeni. Elküldve; Azure Media Services meg automatikusan figyelmen kívül hagyja. [Csatorna alaphelyzetbe](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) használatával a ingest pont állapot alaphelyzetbe állítása az ajánlott és is javasolt [Program leállítása](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) használata egy bemutatóban és a adatfolyam befejezéséhez.
6. MP4 fragment időtartamának kell állandó, az ügyfél-jegyzékfájlok méretének csökkentése és-ügyfél letöltési heurisztikus ismétlési címkék segítségével javítható.  Az időtartam annak érdekében, hogy nem egész keret díjak kompenzálja ingadozhat.
7. MP4 fragment időtartamának körülbelül 2 és 6 másodperc között kell lennie.
8. MP4 fragment időbélyegeket és az indexek (TrackFragmentExtendedHeaderBox fragment_ absolute_ idő és fragment_index) kell kapják meg üzeneteiket az növekvő sorrendbe.  Bár az Azure Media Services rugalmassá az ismétlődő töredékek, nagyon van-e azt jelenti, hogy az media ütemterv szerint töredékek átrendezése korlátozott.

##<a name="4-protocol-format--http"></a>4. Protocol (protokoll) formátumban – HTTP

ISO tördelt MP4 élő alapú HTTP POST hosszan futó szabványos küldött csomagolt tördelt MP4 formátumban a szolgáltatás kódolt multimédia-adatok kérése a Microsoft Azure Media Services felhasználási ingest. Minden egyes HTTP POST küld egy teljes tördelt MP4 bit-adatfolyam ("adatfolyam") kezdve élőfej jelölőnégyzetét ("ftyp", "Élő kiszolgáló cikkét mezőbe" és "moov" mező) vagy egy folytatása sorozatát töredékek ("moof" és "mdat" mező esetén). HTTP POST kérelem szintaxisa URL-cím, nézze meg [1] 9.2 lévő szakaszra. A következő példa a KÜLDÉS URL-címe: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Követelmények

Az alábbiakban a részletes követelményeket:

1. Kódoló a közvetítés CÉLSZERŰ először egy üres "szervezethez" (nulla tartalmának hossza) az azonos bevitel URL-cím használatával HTTP POST felkérés küldése. Ez segít gyorsan észleli, ha az élő bevitel végpontot érvényes, és nem minden hitelesítés vagy egyéb feltételekkel szükséges. / HTTP protokollt a kiszolgáló nem küldhet vissza HTTP-válasz mindaddig, amíg a teljes kérés, beleértve a bejegyzés törzsébe. Élő eseményeket, ebben a lépésben nélkül időigényes jellegét megadott a kódoló nem lehet olyan hibák feltárása, amíg be nem fejezi küldése az összes adatot.
2. Kódoló esetleges hibákat vagy hitelesítési kihívásokkal (1) eredményeként kell kezelni. Ha (1) sikerül 200 választ, továbbra is.
3. Kódoló új HTTP POST kérelem kell kezdődnie a töredezett MP4-adatfolyam.  A tartalom a fejléc mezőkbe töredékek követ kell kezdődnie.  Megjegyzés: a "ftyp", "Élő kiszolgáló cikkét mezőbe" és "moov" mezője (ebben a sorrendben) minden összehívással lehet küldeni, még akkor is, ha a kódoló csatlakoznia kell, mert az előző kérelem az adatfolyam vége előtt megszakadt. 
4. Kódoló kell használnia darabolásos átadása kódolást feltöltése, mivel előre, az élő esemény teljes tartalmának hossza nem lehetséges.
5. Ha az esemény keresztül, a legutóbbi részlet elküldése után a kódoló biztonságosan végén szerepelnie kell a darabolásos átadása kódolást üzenet sorrend (a legtöbb HTTP ügyfél készleteket kezelje automatikusan). Kódoló várja meg a szolgáltatás vissza a végső válasz kódot, és ezután megszünteti a kapcsolatot. 
6. A Events() főnévi kódoló nem kell használata a Microsoft Azure Media Services élő szempontjából 9.2 a [1] leírt módon.
7. HTTP POST kérése ér véget, vagy az adatfolyam TCP hibával vége előtt időtúllépés történik, ha a a kódoló kell ki egy új bejegyzés kérelmet új kapcsolaton keresztül, és kövesse a feletti követelményeinek további kötelező, hogy a kódoló kell küldje el az egyes nyomon követése az adatfolyamban az előző két MP4 töredékek, és folytassa az media idővonalon folytonosság megszakítását bemutatása nélkül.  Az utolsó két MP4 töredékek az egyes számok újraküldése biztosítja, hogy van-e az adatvesztés.  Más szóval ha adatfolyam tartalmaz egy hang- és követése, és az aktuális bejegyzés kérelem nem sikerült, a kódoló kell újracsatlakozáshoz, és küldje el az utolsó két töredékek esetében a hangsáv, amely a korábban már sikerült küldtek, és a videó zeneszám az utolsó két töredékek, amely a korábban már sikerült küldtek, annak érdekében, hogy győződjön meg arról, hogy van-e az adatvesztés.  A kódoló kell karbantartása: a "határidős" puffer media töredékek, amely, amikor újracsatlakozás újraküldi.

##<a name="5-timescale"></a>5. időskála 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) SmoothStreamingMedia (szakasz 2.2.2.1), StreamElement (szakasz 2.2.2.3), StreamFragmentElement(2.2.2.6) és LiveSMIL (szakasz 2.2.7.3.1) a "Időskála" használatát ismerteti. Időskála értéke nem bemutató, a használt alapértelmezett érték esetén 10,000,000 (10 MHz). Bár a zökkenőmentes Streaming formátummegadás nem blokkolja a más időskála érték, a legtöbb kódoló megvalósítás felhasználási használatát ez az alapértelmezett érték (10 MHz) létrehozása adatok zökkenőmentes Streaming ingest. Miatt az [Azure Media dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md) szolgáltatás célszerű használni szeretné a video-adatfolyam megjelenítését 90 kHz időskála és 44,1 vagy 48.1 kHz hang adatfolyamok ajánlott. Ha másik időskála értékek másik adatfolyamok használ, az adatfolyam szintű időskála kell elküldeni. Nézze meg [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. "Adatfolyam" meghatározása  

"Adatfolyam" élő bemutató szerkesztési, a kezelés adatfolyam feladatátvevő és redundancia esetek élő bevitel művelet alapegység értéke. "Adatfolyam" egy egyedi tördelt MP4 bit-adatfolyam amelyek tartalmazhatnak egy egyetlen követése vagy több zeneszám definíciója. Egy teljes élő bemutató tartalmazhatnak egy vagy több adatfolyamok az élő encoder(s) konfigurációjától függően. Az alábbi példák bemutatják, stream(s) használata a bemutató teljes élő írhat különböző beállításokat.

**Példa:** 

Ügyfélnek nem felel meg az alábbi hang-és videoadat-bitrates tartalmazó élő adatfolyam bemutató létrehozása:

Videó – 3000 KB, 1 500 KB, 750 KB

Hang – 128 KB

###<a name="option-1-all-tracks-in-one-stream"></a>1 beállítást: Minden szám egy adatfolyamban

Ezt a lehetőséget egy egyetlen encoder összes hang-és videoadat-s zeneszámok hoz létre, és foglalja csomagba őket egy tördelt MP4 bit-adatfolyam majd egyetlen HTTP POST kapcsolaton keresztül kapja küldő be. Ebben a példában az élő adás csak egy falának van:

![image2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>2 lehetőség: Minden nyomon követése egy külön adatfolyamban

Ezt a beállítást, a csak tegye a encoder(s) be minden részlet MP4 átviteli adatfolyam nyomon tanulmányozása és kérdések feltevése a adatfolyam megjelenítését több külön HTTP-kapcsolaton keresztül Ez lehet elvégezni kódolók egy vagy több kódolók. Élő bevitel szempontjából az élő adás adatfolyamok négy tevődik össze.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>3 beállítás: Az első lépésekhez hang nyomon követése a legalacsonyabb átviteli sebesség videó nyomon követése egyetlen adatfolyam-be

Ezt a beállítást, az ügyfél úgy dönt, hogy foglalja csomagba a hangsáv a legalacsonyabb átviteli sebesség videó nyomon követése a több részlet MP4 átviteli adatfolyam tartalmazó mezőt pedig hagyja a másik két videó számok saját adatfolyam alatt. 


![image4][image4]


###<a name="summary"></a>Összefoglalás

Fent látható a teljes körű lista nem az ebben a példában az összes lehetséges bevitel beállítást. Számok adatfolyamok be minden olyan csoportja, valójában, az élő bevitel által támogatott. A vevők és encoder szállítók saját megvalósítás mérnöki összetettsége, encoder beosztását, és a redundancia és feladatátvevő szempontok alapján beállíthatja. Megjegyzendő azonban, hogy a legtöbb esetben van a teljes élő bemutató csak egy hangsáv, fontos, hogy a egészséges legyen, amely tartalmazza a hangsáv ingest adatfolyam biztosítása. Gyakran a figyelmet a hangsáv üzembe helyezése a saját adatfolyam (például beállítás 2), vagy azt a legalacsonyabb átviteli sebesség videó nyomon követéséhez (például beállítás 3) a hozzákapcsolása eredményez. Még hatékonyabb redundancia és hibatűrést ugyanazt a hangsáv elküldésének két különböző adatfolyamok (felesleges zeneszámok beállítás 2) vagy a hangsáv legalább két legalacsonyabb átviteli sebesség videó számok (beállítás 3 kapcsolt legalább két video-adatfolyam megjelenítését a hanggal) erősen ajánlott élő hozzákapcsolása ingest be a Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. feladatátvevő szolgáltatás 

Élő streaming jellegét, az adott jó feladatátvevő támogatási fontos biztosítása érdekében a szolgáltatást az elérhető. Microsoft Azure Media Services különböző típusú hibákat, például hálózati hibák, a kiszolgáló hibák, a tárhely problémák, a stb kezelésére szolgál. Élő encoder oldaláról megfelelő feladatátvevő logika együtt használva ügyfél a felhőből nagymértékben megbízható élő adatfolyam szolgáltatás érhet el.

Ebben a részben ölelik fel szolgáltatás feladatátvevő esetek lesz. Ebben az esetben a hiba történik valahol a szolgáltatás belül, és hálózati hiba jelentkezik. Az alábbiakban néhány javaslatot szolgáltatás feladatátvevő kezelésére szolgáló encoder végrehajtása:


1. A TCP-kapcsolat létrehozása a 10 második időtúllépése használatával.  Ha valaki megpróbálja a csatlakozást tovább tart, mint 10 másodperc, megszakítja a műveletet, és próbálkozzon újra. 
2. Rövid időtúllépés küldéséhez a HTTP-kérés üzenet szövegadattömb használni.  Ha a cél MP4 fragment időtartam N másodperc, akkor a Küldés időtúllépése N és 2N másodperc; között például használ 6 – 12 másodperc időtúllépés, ha a MP4 fragment időtartam 6 másodperc.  Időtúllépés történik, ha a kapcsolat alaphelyzetbe állítása, nyisson meg egy új kapcsolatot és önéletrajz adatfolyam ingest az új kapcsolat. 
3. Naplók karbantartása: a működés közben puffer tartalmazó az utolsó két, az egyes nyomon követése küldött sikeresen és teljesen a szolgáltatást.  Ha adatfolyam HTTP POST vonatkozó megszakad vagy időtúllépés történik előzetes az adatfolyam végére nyisson meg egy új kapcsolatot, és kezdje el a másik HTTP POST kérelem, küldje el a adatfolyam fejlécek, küldje el az egyes nyomon követése az utolsó két töredékek, és folytassa az adatfolyam anélkül, hogy a folyamatosság megszakadása az media idővonalon bemutatása.  Ez csökkenti az adatok elvesztését lehetőségét.
4. Javasoljuk, hogy a kódoló nem korlátozza az kapcsolatot létesíteni, és folytathatja a folyamatos átvitelű után a TCP-hiba lép fel ismétlések számát.
5. Miután a TCP-hiba:
    1. Az aktuális kapcsolathoz kell zárni, és új kapcsolatot, létre kell hozni az új bejegyzés HTTP összehívást.
    2. Az új HTTP bejegyzés URL-címet kell ugyanaz, mint az első bejegyzés URL-címet.
    3. Az új HTTP bejegyzés szerepelnie kell adatfolyam fejlécek ("ftyp", "Élő kiszolgáló cikkét mezőbe" és "moov" mező) megegyezik az adatfolyam fejlécek az indító bejegyzés.
    4. Az egyes számok küldött utolsó két töredékek kell újra el kell küldenie, és bemutatása a folyamatosság megszakadása az media idővonalon nélkül a folyamatos átvitelű folytatódik.  A MP4 fragment időbélyegeket folyamatosan növelnie kell akár HTTP POST kérések keresztül.
6. A kódoló kell ér véget a HTTP-bejegyzés kérelmet, ha az adatok nem küldi a MP4 fragment időtartam arányos sebessége.  Nem küld adatokat a HTTP-bejegyzés felkérés megakadályozhatja, hogy Azure Media Services gyorsan leválasztása a kódoló megelőzve a szolgáltatás frissítését.  Emiatt a HTTP-BEJEGYZÉST a ritka (Active Directory-jel) számok kell lennie rövid életben volt végződő, amint a ritka részlet küldi.

##<a name="8-encoder-failover"></a>8. feladatátvevő encoder

Kódoló feladatátvevő feladatátvevő forgatókönyv kell foglalkozni, a végpontok közötti élő adatfolyam kézbesítésre, hogy a második típusú. Ebben az esetben a hiba okát encoder oldalán történt. 

![image5][image5]


Kódoló feladatátvevő történik, amikor az alábbiakban a várakozásoknak, az élő bevitel végpontot a:

1. Új encoder példányt a folyamatos átvitelű, a folytatáshoz hozható létre amint a fenti diagramot (3000 k szaggatott vonal video-adatfolyam).
2. Az új encoder kell használni az azonos URL-cím HTTP POST kérések sikertelen példányt.
3. Az új encoder POST kérelem azonos töredezett MP4 élőfej megfelelően a jelölőnégyzeteket a sikertelen példány kell tartalmaznia.
4. Az új encoder szinkronizálni kell a megfelelő és minden más futó kódolók az élő ugyanabban a bemutatóban igazított fragment határai szinkronizált hang-és videoadat minták létrehozásához.
5. Az új adatfolyam listamezőben megegyezik a következővel: az előző adatfolyam és cserélhető kell lennie az élőfej- és fragment szintjén.
6. Az új encoder próbálja meg az adatok elvesztését összezárása.  A fragment_absolute_time és a multimédiás töredékek fragment_index növelni kell az a pont, ahol a kódoló utolsó leállt.  A fragment_absolute_time és fragment_index növelni kell a folyamatos módon, de a megengedett a folyamatosság megszakadása bevezetésére, ha szükséges.  Azure Media Services figyelmen kívül hagyja töredékek, akkor már fogadott és feldolgozása, így még jobb szélén, mint a bevezetése a médiafájlok idősorban folytonosság megszakítását töredékek újraküldése hibaüzenet. 

##<a name="9-encoder-redundancy"></a>9. a redundancia encoder 

Az egyes kritikus élő eseményeket, hogy igény szerint még magasabb rendelkezésre állás és élmény minősége, célszerű alkalmaznia aktív-aktív felesleges kódolók zökkenőmentes feladatátvevő adatvesztés nélkül adatok eléréséhez.

![image6][image6]

Amint a fenti diagramot, vannak minden adatfolyam két példánya egyidejű terjesztése az élő üzembe kódolók két csoportját. Ez a beállítás funkció használható, mivel a Microsoft Azure Media Services rendelkezik, az azt jelenti, hogy ki ismétlődő töredékek adatfolyam Azonosítóját és fragment időbélyeg alapján szűrni. Az eredményül kapott élő adatfolyam és archív mappák a adatfolyam megjelenítését, amely a két forrásból származó legjobb lehet összesítést példányának egyetlen lesz. Például a hipotetikus szélső esetben addig, amíg egy kódoló (nem szükséges lehet azonos) időpontra és az egyes adatfolyam fut minden megadott helyen, a szolgáltatásból az eredményül kapott élő adatfolyam folyamatos adatvesztés nélkül lesz. 

Ebben az esetben kötelező megegyezik szinte Encoder feladatátvevő jelenik meg a futó kódolók második csoportja az elsődleges kódolók egyszerre kivételével igényeivel.

##<a name="10-service-redundancy"></a>10. a redundancia szolgáltatás  

Nagyon felesleges globális eloszlás időnként szükség van, hogy az idegen-terület biztonsági másolat területi katasztrófák kezelheti. Kattintson a "Kódoló redundancia" topológia kibontása, ügyfelek megadhatja a felesleges szolgáltatás telepítése egy másik régióbeli, mely kódolók 2nd csoportja. Ügyfelek sikerült is dolgozhat a CDN-szolgáltató egy GTM (globális forgalom kezelő) üzembe ügyfél zökkenőmentes forgalmat a két szolgáltatás telepítések elé. A kódolók követelményei ugyanaz, mint a második készlete kódolók kell kell emelni, egy másik élő csak kivétellel "Kódoló redundancia" eset ingest végpont. Az alábbi ábrán látható ez a beállítás:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. bevitel formátumok típusú speciális 

Ez a szakasz ismerteti, hogy bizonyos speciális típusú élő bevitel formátumok, amelyek bizonyos bizonyos esetekben kezelheti.

###<a name="sparse-track"></a>A ritka nyomon követése

Ügyfélprogramok élményt élő adatfolyam bemutató előadása során gyakran szükség, események idő szinkronizált küldött, vagy sávon a fő multimédia-adatok a jeleket. Ez példája dinamikus élő hirdetések beszúrási. A következő típusú esemény-jelzés normál hang-és videoadat miatt a ritka természeti streaming különbözik. Más szóval jelzőcsatorna adatok általában nem történik folyamatosan, és az intervallum előrejelzésére nehéz lehet. Amely a ritka követése ingest és közvetítése sávon jelzőcsatorna adatok kifejezetten volt.

Az alábbi ajánlott végrehajtására ingesting ritka nyomon követése a következő:

1. Hozzon létre külön tördelt MP4 bit-adatfolyam nélkül hang-és videoadat-s zeneszámok ritka szám csak tartalmazó.
2. A "élő kiszolgálója cikkét mezőben" meghatározott a 6 [1] "parentTrackName" a paraméter használatával adja meg a nevét a szülő nyomon követése. Szakasz 4.2.1.2.1.2 [1] további részletekért olvassa el.
3. A "élő kiszolgáló cikkét mezőben" manifestOutput meg kell "igaz".
4. Adott jelzőcsatorna esemény ritka jellegét, azt ajánljuk, hogy:
    1. Az élő esemény elején encoder elküldi a kezdeti élőfej mezőben a szolgáltatás, amely lehetővé teszi a szolgáltatást, hogy az ügyfél jegyzék rögzítheti a ritka nyomon követése.
    2. A kódoló kell ér véget a HTTP POST kérelem, amikor nem küldi.  A hosszú futó HTTP-bejegyzés, amely nem küld adatokat megakadályozhatja, hogy Azure Media Services gyorsan leválasztása a kódoló megelőzve a szolgáltatás frissítése vagy a kiszolgáló újra kell indítani, szerint a kiszolgáló ideiglenesen azzal blokkolhatja a szoftvercsatornáján fogadási művelet. 
    3. Alatt az illetőknek, amikor jelzőcsatorna adatok nem érhető el a kódoló SHOULD zárja be a HTTP-bejegyzés kérhet.  A bejegyzés kérelem beállítás aktív, miközben a kódoló küldjön adatok 
    4. A ritka töredékek küldésekor encoder szerint megadhatja explicit tartalom hosszúságú élőfej érhető el.
    5. Az új kapcsolat ritka fragment küldésekor encoder kezdetének a élőfej jelölőnégyzetét, majd az új töredékek küldését. Ez a kezelheti az esetben, ha feladatátvevő történt a kettő között a ritka új kapcsolat jön létre egy új kiszolgálóhoz, amely nem tartalmaz benne a ritka követése előtt.
    6. A ritka követése részlet lesz elérhető az ügyfél számára a megfelelő szülő nyomon részlet, amelynek egyenlő vagy nagyobb időbélyeg értéket az ügyfél számára hozzáférhető. Például ha a ritka részlet t időbélyeg = 1000, miután az ügyfél által látott nézetet (feltételezve, hogy a szülő követése neve (videó)) a videó részlet időbélyeg 1000, vagy túl, töltse le a ritka részlet t várhatóan = 1000. Felhívjuk a figyelmét arra, hogy a tényleges jel nagyon jól használható a bemutató idősorban egy másik helyre a kijelölt célra. A fenti példában is lehet, hogy a ritka részlet t = 1000 van egy XML-tartalom beszúrására, az Active Directory abban a helyzetben, amely néhány másodpercet vagyis később.
    7. A tartalom a ritka követése fragment lehet attól függően, hogy a különböző forgatókönyvek számos különböző formátumokban (pl. XML vagy szöveg vagy bináris, stb.). 


###<a name="redundant-audio-track"></a>Felesleges hang nyomon követése

Egy tipikus HTTP adaptív Streaming esetben (pl. zökkenőmentes Streaming vagy szaggatott) nem gyakran csak egy hangsáv a teljes bemutatóban. Az ügyfél hiba feltételek közül választhat, többszintű minőség amelyeknek videó számok eltérően a hangsáv lehet egyetlen hiba pont Ha megszakad a bevitel, amely tartalmazza a hangsáv adatfolyam. 

A probléma megoldásához a Microsoft Azure Media Services támogatja a felesleges hang felvételek élő bevitel. A funkció lényege, hogy ugyanazt a hangsáv elküldésének többször a különböző adatfolyam megjelenítését. A szolgáltatás csak regisztrálja a hangsáv egyszer be az ügyfél jegyzék, azt is használhatja a felesleges zeneszámok biztonsági mentés másként számára beolvasása hang töredékek, ha az elsődleges hangsáv van problémákat. Annak érdekében, hogy a felesleges zeneszámok ingest, a kódoló kell:

1. A több részlet MP4 átviteli adatfolyam ugyanazt a hangsáv létrehozása A felesleges hang számok listamezőben megegyezik a következővel: pontosan a azonos fragment időbélyegeket és cserélhető kell lennie az élőfej- és fragment szintjén.
2. Győződjön meg arról, hogy a "hang" bejegyzést a Live kiszolgáló cikkét (szakasz 6 [1]) ugyanazok, az összes felesleges zeneszámok.

Az alábbi képen egy felesleges zeneszámok ajánlott végrehajtására:

1. Küldjön minden egyedi hangsáv adatfolyam önmagában.  Is küldhet egy felesleges adatfolyam minden egyes e hangsáv adatfolyamok, ahol a 2nd adatfolyam tér el a 1st csak a HTTP-bejegyzés URL-CÍMÉT az azonosító: {protokoll} :// {kiszolgáló címét} / {közzétételi pont path}/Streams({identifier}).
2. Külön adatfolyamok funkcióval a legkisebb két videó bitrates küldhet. Minden egyes e adatfolyamok minden egyedi hangsáv másolatának is tartalmazhat.  Több nyelv használata támogatott, ha például e adatfolyamok tartalmaznia kell zeneszámok az egyes nyelvekhez.
3. Külön server (kódoló)-példányok segítségével kódolását, és küldje el a felesleges adatfolyamok említett (1) és (2). 



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 