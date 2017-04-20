# <a name="internet-of-things-security-architecture"></a>A dolog az Internet biztonsági architektúrája

A rendszer tervezésekor fontos megérteni, hogy a rendszer a potenciális veszélyek, és ennek megfelelően hozzá a megfelelő védelmet, a rendszer tervezett és tervezett. Különösen fontos kezdetétől a termék tervezése biztonság szem előtt, mert ismertetése, hogyan a támadó esetleg veszélyeztetheti a rendszer segítségével győződjön meg arról, hogy megfelelő megoldásokkal kapcsolatban állnak az elejétől kezdve. 

## <a name="security-starts-with-a-threat-model"></a>Biztonsági fenyegetés modell kezdődik.
 
A Microsoft termékei fenyegetés modellek hosszú használta, és tett a vállalat fenyegetést a modellezési folyamat nyilvánosan elérhető. A vállalat tapasztalat bizonyítja, hogy rendelkezik a modellezés fenyegetések Mik a legtöbb azonnali megértése túl nem várt előnyök kapcsolatos. Például is létrehoz egy avenue egy nyitott vitafórum másokkal az új ötletek, és a termék javítását vezethet fejlesztőcsoport kívül.
  
Fenyegetés modellezési célja rámutatni, hogy a támadó esetleg behatolhatnak a rendszerbe, és ellenőrizzük, hogy megfelelő megoldásokkal kapcsolatban állnak. A Tervező csapat megoldásokkal kapcsolatban figyelembe kell venni, mint a rendszer úgy lett kialakítva, hanem után a rendszer fenyegetést modellezési erők telepítve van. Ezt a tényt kritikusan fontos azért, mert a biztonsági védelmet az eszközök területén számtalan átépítését lehetetlen, hajlamos a hiba, és hagyja a vevők kockázatnak.

Számos fejlesztői csapat munkaköre egy kiváló rögzítése a rendszer funkcionális követelményeit, amelyek a fogyasztók javát. Azonban, hogy valaki visszaél előfordulhat, hogy a rendszer nem nyilvánvaló módon azonosítására is megnehezítheti. Fenyegetés modellezési segít megérteni a támadó esetleg mit fejlesztői csapat és miért. Fenyegetés modellezési egy strukturált folyamat, amely a rendszer tervezési döntéseket hoz a biztonsággal kapcsolatos vitát, valamint a menet végzett e hatás biztonsági terv módosítása. Bár fenyegetés modell egyszerűen egy dokumentumot, ez a dokumentáció is képviseli Tudásbázis retenciós tanulságok folyamatosságát megtanult, és új súgó gyorsan csapat fedélzeti kiváló lehetőséget. Végezetül egy fenyegető modellezési eredményét ahhoz, hogy figyelembe kell venni más szempontokat biztonsági intézkedést, például milyen biztonsági kötelezettségvállalások kíván biztosítani az ügyfelek. Ezek a kötelezettségvállalások fenyegetés modellezési együtt tájékoztatja, és a meghajtó vizsgálata a dolog Internet (IoT) oldat.
 

### <a name="when-to-threat-model"></a>Ha a modell veszélyt

[Fenyegetés modellezés](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) kínálja a legnagyobb érték bekerül a tervezési szakaszban. Ha tervez, akkor változtatásokat veszélyek kiküszöbölésére a nagyobb rugalmasság érdekében. A kívánt eredmény veszélyek kiküszöbölésére szándékosan. Akkor sokkal egyszerűbb, mint hozzáadása megoldásokkal kapcsolatban, kipróbálása és aktuális maradjanak, és ezen túlmenően az ilyen eltávolítás nem mindig lehetőség biztosítása. Veszélyek kiküszöbölésére egy termék több érett lesz, és több munkát és sokkal nehezebb, mint fenyegetés fejlesztése során a korai modellezési mellékhatásokkal viszont végül igényel nehezebb lesz.

### <a name="what-to-threat-model"></a>Mi a fenyegetés modell

Szál modell egészének az oldatot kell, és is elsősorban az alábbi területeken:

- A biztonsági és adatvédelmi szolgáltatások
- Amelynek hibák a megfelelő biztonsági szolgáltatások
- Érintse meg a megbízhatósági határ szolgáltatások 

### <a name="who-threat-models"></a>Aki veszélyt modellek

Fenyegetés modellezési más folyamat.  Célszerű kezelni, mint a többi része a megoldás a fenyegetés forrásmodell-dokumentumba, és ellenőriznie. Számos fejlesztői csapat munkaköre egy kiváló rögzítése a rendszer funkcionális követelményeit, amelyek a fogyasztók javát. Azonban, hogy valaki visszaél előfordulhat, hogy a rendszer nem nyilvánvaló módon azonosítására is megnehezítheti. Fenyegetés modellezési segít megérteni a támadó esetleg mit fejlesztői csapat és miért.

### <a name="how-to-threat-model"></a>Hogyan fenyegetés modell

A modellezési folyamat veszély áll négy lépés; a lépések a következők:

- Minta alkalmazása
- Fenyegetések felsorolása
- Veszélyek csökkentése
- A enyhítését ellenőrzése

#### <a name="the-process-steps"></a>A folyamat lépéseinek

Három szabályok a legjobb megoldás fenyegetés modell létrehozásakor ne feledje, hogy:

1. Nincs referencia-architektúra diagram létrehozása. 
2. Indítsa el a szélessége-első. Áttekintésére, és tisztában van a rendszer a teljes mély merülés előtt.  Ez lehetővé teszi, hogy Ön mély-merülési a megfelelő helyen.
3. A folyamat meghajtó, ne engedje, hogy a meghajtó, akkor a folyamat. Ha hibát talál a modellezési fázis, és szeretné megtalálni azt, keresse meg!  Nem érzi slavishly kövesse az alábbi lépéseket kell.  

#### <a name="threats"></a>Fenyegetések

A négy legfontosabb fenyegetés modell elemei:

- Folyamatok (webes szolgáltatások, a Win32 szolgáltatások, * nix démonok, stb. Vegye figyelembe, hogy néhány összetett entitás (pl. mező átjárók és érzékelők) térfogatát, mint egy folyamat, amikor egy műszaki Leásás ezeken a területeken nincs lehetőség.
- Adatokat tárol (tetszőleges helyre adatokat tárolja, például a konfigurációs fájlban vagy adatbázisban)
- Adatfolyam (ahol adatok mozgatja az alkalmazás más elemek között)
- Külső (semmit kommunikál a rendszer, de ez nem az alkalmazás ellenőrzése alatt, példák közé tartozik a felhasználók és a műholdas hírcsatornák)

Minden eleme az architektúra ábrája vonatkoznak a különböző veszélyek; a LÉPÉSKÖZ rövidítés azt fogja használni. [Fenyegetés modellezés újra, LÉPÉSKÖZ](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) többet tudni a LÉPÉSKÖZ elemek olvasása.

Az alkalmazás diagram különböző elemeinek egyes LÉPÉSKÖZ fenyegetések vonatkoznak:

- Folyamatok LÉPÉSKÖZ vonatkoznak.
- Adatfolyamok TID vonatkoznak.
- Adattárak is TID és néha R, ha az adattárolók naplófájlok.
- Külső SRD vonatkoznak.

## <a name="security-in-iot"></a>Biztonsági IoT

Speciális csatlakoztatott eszközök rendelkezik jelentős számú potenciális beavatkozás területére és a kölcsönhatás minták, amelyek keretet biztosítani az eszközök digitális hozzáférés biztosítása érdekében figyelembe kell venni. A "digital access" kifejezés itt megkülönböztetni a közvetlen beavatkozás keresztül végrehajtott műveletek hozzáférés biztonsági amennyiben a fizikai hozzáférés-szabályozás révén. Például üzembe helyezése az eszköz zár helyiség az ajtóra. Fizikai hozzáférés nem tagadható meg szoftver és hardver, miközben vezető rendszer interferencia fizikai hozzáférés megakadályozása érdekében intézkedéseket lehessen hozni. 

Mint azt a kapcsolatitevékenység-mintázatok tájékozódáshoz azt fog kinézni "eszközvezérlő" és "eszköz adatok" ugyanakkora figyelmet. "Vezérlő eszköz" minden olyan információt, bármely fél által biztosított eszközök, módosítása vagy felé az állam vagy a környezet állapotáról a viselkedését befolyásoló céllal is minősíthető. "Eszköz adatok" akkor nevezhető minden olyan információt, az eszköz állapotát és a környezet állapotának megfigyelt bármely másik fél bocsát ki.
   
Annak érdekében, hogy optimalizálja a biztonsággal kapcsolatos gyakorlati tanácsok, ajánlott, hogy egy tipikus IoT-architektúra több összetevő/zónákra osztását részeként a fenyegetés modellezés gyakorlására. Ezek a zónák teljesen ebben a szakaszban leírt, és tartalmaznia kell:

-   Eszköz,
-   A mező átjáró
-   Felhő alapú átjárókat, és
-   Szolgáltatások.

Zónák átfogó rendszer módon oszthatja fel egy megoldást. az egyes zónák gyakran van saját adatok és a hitelesítési és engedélyezési követelményeknek. Zónák használható elkülönítési károk és korlátozhatja a magasabb megbízhatósági zónák zónák alacsony bizalmi hatását is.

Az egyes zónák egy megbízhatósági határ, ami kell jegyezni, mint az alábbi ábrán szaggatott piros vonal választja el egymástól. Azt átmenetet jelöl, az adatok/információk több forrásból a másikra. Az áttérés során az adatok információt lehet tartalomhamisítást lehetővé, Tampering, megtagadás, adatokhoz való illetéktelen hozzáférést, szolgáltatásmegtagadást és jogosultság illetéktelen megszerzését (LÉPÉSKÖZ).

![IoT biztonsági zónák](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Az összetevők minden határán belül kitaláltak is vannak kitéve a LÉPÉSKÖZ, amely lehetővé teszi egy teljes 360 nézet oldatot modellezés fenyegetést. Az alábbi szakaszok ismertetik az egyes összetevők és bizonyos biztonsági problémákat és megoldásokat kell bevezetni.

A következő szakaszok megvitatja általában ebben a zónában található szabványos összetevőivel.

### <a name="the-device-zone"></a>Az eszköz zóna

Az eszköz környezet az eszköz fizikai azonnali körülötte adott fizikai hozzáférést és/vagy "helyi hálózat" társ-társ digitális hozzáférést az eszközhöz is megvalósítható. "Helyi hálózat"-nak tekinti a különböző és – a szigetelt, de potenciálisan Áthidalt és – a nyilvános Internet, és magában foglalja a rövid hatótávolságú vezeték nélküli rádiós technológia, amely lehetővé teszi a társ-társ kommunikációs eszközök a hálózat. Igen *nem* minden hálózati virtualizációs technológia létrehozása egy helyi hálózaton illúzióját tartalmazzák, és nem is szerepelnek a közszolgáltató hálózatokat, amelyekhez bármely két eszköz hely nyilvános hálózaton keresztül tudnak kommunikálni, ha a társ-társ kommunikációs kapcsolatot adja meg.

### <a name="the-field-gateway-zone"></a>A mező átjáró zóna

A mező az átjáró az eszköz/berendezés vagy néhány általános célú kiszolgáló számítógépes szoftver, amely kommunikációs engedélyezője, és esetleg adatfeldolgozó hub eszközt és ellenőrzési rendszer eszköz működik. A mező átjáró tartozik a mező átjáró maga és minden hozzá kapcsolódó eszközök. Ahogy a neve is jelzi, mező átjárók külső dedikált adatfeldolgozási berendezések jár el, általában helyhez kötött, fizikai behatolás potenciálisan vonatkoznak, és működési redundancia korlátozott lesz. Minden, mondja ki, hogy a mező az átjáró az általában egy dolog egy érintés és szabotálására közben ismerete funkciója van. 

Mező átjáró eltér a puszta forgalmat útválasztó, hogy aktív szerepet hozzáférés-kezelés volt, és információáramlás, ami azt jelenti, az alkalmazás címzett személy és a hálózati kapcsolat vagy a Terminálszolgáltatások munkamenet. A NAT-eszköz vagy a tűzfal, ezzel szemben nem minősülnek mező átjárók mivel nem egyértelmű kapcsolat vagy munkamenet-terminálok, de inkább egy útvonalat (vagy blokk) kapcsolatok vagy rajtuk keresztül végzett munkamenetek. A mező átjáró két különböző felszíni terület van. Egy néz az eszközök, amelyek kapcsolódnak, és annak a zónának a belső képviseli, és minden külső fél néz, és a zóna szélére a másik.   

### <a name="the-cloud-gateway-zone"></a>A felhő átjáró zóna

Felhő átjáró egy olyan rendszer, amely lehetővé teszi a távoli kommunikációt eszközök vagy mező átjárók különböző helyekről és keresztül nyilvános hálózati helyre, általában egy felhőalapú ellenőrzési és elemző rendszer, az ilyen rendszerek egy összevonási felé. Bizonyos esetekben felhő átjáró azonnal megkönnyíthetik access speciális eszközöket a terminálok tabletták vagy telefonokhoz. Az itt tárgyalt összefüggésben a "felhő" hivatkozik a dedikált adatfeldolgozó rendszer ugyanazon a helyen, mint a csatolt eszközökről vagy mező átjárók nem kötött kell érteni. Is egy felhő zónában operatív intézkedések célzott fizikai hozzáférés megakadályozása, és nem feltétlenül érhető el a "nyilvános felhő" infrastruktúra.  

Felhő átjáró potenciálisan társítja a virtualizálás hálózati átfedés és az összes csatlakoztatott eszközök vagy a mező átjárók a hálózati forgalmat a felhő átjáró háromszintű. A felhő átjáró, önmagában egy sem eszköz ellenőrző rendszer, sem a feldolgozó létesítmény vagy tárolóhely eszköz adatok; Ezekben a létesítményekben felület felhő átjáró. A felhő átjáró a felhő átjáró maga mező átjárók és a kapcsolódó gyermekeszközt közvetlenül vagy közvetetten tartozik. A zóna a széle pedig külön terület ahol minden külső felek keresztüli kommunikáció.

### <a name="the-services-zone"></a>A szolgáltatások zóna

A "szolgáltatás" van definiálva ebben az összefüggésben a fürtszoftver-összetevő vagy a modul, amely a vele kapcsolatba kerülő eszközök mező - vagy felhő átjárón keresztül adatok gyűjtése és elemzése, valamint a Hangvezérlés.  Szolgáltatások olyan közvetítők. Azok átjárók és más alrendszerek identitásukat alatt jár el, tárolására és adatok elemzése, autonóm módon eszközök parancsokat adatok elképzelések vagy menetrend alapján és információ elérhetővé és ellenőrzés engedélyezett végfelhasználóknak képességeit.

### <a name="information-devices-vs-special-purpose-devices"></a>Speciális eszközök és információk-eszközök

Személyi számítógépek, telefonok és tabletta, elsősorban interaktív információs eszköz illesztőprogramja. Telefonok és tabletta kifejezetten optimalizált körül a telep élettartamának maximalizálása. Lehetőleg kikapcsolása részben nem azonnal interakció során egy személy, vagy ha nem nyújt szolgáltatásokat, például zenelejátszás vagy irányadó a tulajdonos egy adott helyen. Rendszerek szempontból ezen információ technológiai eszközök főként járnak, proxyként személyek felé. Műveletek és beviteli gyűjtése "emberek érzékelők" javaslat "emberek rákötni". 

Speciális eszközök, az egyszerű hőmérséklet érzékelők összetett gyár termelési sorok összetevők bennük, ezer különböznek. Ezek az eszközök sokkal több keresési tartománya a cél, és akkor is, ha néhány felhasználói felületet biztosítanak, való együttműködésre keresési nagyrészt tartománya vagy integrálni kell a fizikai világban eszközök. Azok mérésére és jelentést környezeti körülmények, szelepek kapcsolja, servos szabályozása, riasztások hang, váltás a fények és sok egyéb feladatok végrehajtását. Segítenek munkát, amelyhez az információ eszköz túl általános, túl drága, túl nagy vagy túl rideg. A konkrét célra azonnal szabja meg a műszaki tervezési, gyártási és tervezett élettartama alatt a művelet is rendelkezésre álló pénzügyi költségvetési. Ez a két legfontosabb tényező kombinációja korlátozza a rendelkezésre álló működési költségvetés energia, fizikai helyigénye, így elérhető tárolási compute és biztonsági funkciókkal.  

Ha valamit "goes rossz" automatizált és távoli vezérelhető eszközök, például fizikai hibák vagy a vezérlő logikai hibák willful illetéktelen behatolásoktól és a manipuláció. A gyártási tételek meg kell semmisíteni, épületek looted vagy írás és emberek lehet sérült vagy akár die. Ez az, természetesen teljesen eltérő osztály károk, mint valaki maxing ki a lopott hitelkártya korlátot. A biztonsági sáv áthelyezése dolog alkotó eszközök és parancsok, amelyek miatt a dolgok szeretné helyezni, végül eredményező érzékelő adatok elektronikus kereskedelmi vagy pénzügyi forgatókönyv magasabbnak kell lennie. 


### <a name="device-control-and-device-data-interactions"></a>Eszközvezérlő és eszköz adatok kölcsönhatások

Speciális csatlakoztatott eszközök rendelkezik jelentős számú potenciális beavatkozás területére és a kölcsönhatás minták, amelyek keretet biztosítani az eszközök digitális hozzáférés biztosítása érdekében figyelembe kell venni. A "digital access" kifejezés itt megkülönböztetni a közvetlen beavatkozás keresztül végrehajtott műveletek hozzáférés biztonsági amennyiben a fizikai hozzáférés-szabályozás révén. Például üzembe helyezése az eszköz zár helyiség az ajtóra. Fizikai hozzáférés nem tagadható meg szoftver és hardver, miközben vezető rendszer interferencia fizikai hozzáférés megakadályozása érdekében intézkedéseket lehessen hozni. 
 
Mint azt a kapcsolatitevékenység-mintázatok tájékozódáshoz azt fog kinézni "eszközvezérlő" és "eszköz adatok" ugyanolyan szintű fenyegetést modellezési során figyelmet. "Vezérlő eszköz" minden olyan információt, bármely fél által biztosított eszközök, módosítása vagy felé az állam vagy a környezet állapotáról a viselkedését befolyásoló céllal is minősíthető. "Eszköz adatok" akkor nevezhető minden olyan információt, az eszköz állapotát és a környezet állapotának megfigyelt bármely másik fél bocsát ki. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Fenyegetés modellezés az Azure IoT referencia-architektúra

A Microsoft Azure IoT a modellezés veszélyt a fent vázolt keretében használja. Az alábbi részben ezért vesszük Azure IoT referencia-architektúra konkrét példája annak bizonyítására, gondolja át, fenyegetés modellezés a IoT és az azonosított kockázatok kezelésére. A mi esetünkben azt azonosítja a fókusz négy fő területei:

-   Eszköz és adatforrás,
-   Szállítási adatok,
-   Eszköz és az esemény feldolgozása, és
-   Bemutató

![Fenyegetés modellezés a Borzas IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Az alábbi ábra a Microsoft fenyegetés modellezési eszköz által használt adatfolyam-Diagram modell segítségével egyszerűsített nézete a Microsoft IoT architektúra biztosítja:

![Fenyegetés modellezés a Azure IoT MS fenyegetés modellezési eszköz használata](media/iot-security-architecture/iot-security-architecture-fig3.png)

Fontos megjegyezni, hogy az architektúra elválasztja az eszköz és az átjáró képességeit. Átjáró eszközök, amelyek a biztonságosabb előny, hogy a felhasználó: képesek kommunikálni a biztonságos protokollok használatával felhő átjáró igénylő általában nagyobb, adatfeldolgozás, hogy egy natív eszköz – például egy termosztát - szolgálhatnak, saját maga. Az Azure szolgáltatások zóna feltételezzük, hogy a felhő átjáró képviseli az Azure IoT Hub szolgáltatást.

### <a name="device-and-data-sourcesdata-transport"></a>A közlekedési eszközt és az adatforgalmi források/adatok

Ez a szakasz felderíti a fenyegetés modellezési lencséin keresztül fent körvonalazott architektúra, és hogyan azt éppen aktuális néhány együtt járó problémákat áttekintést. A fenyegetés modell fő elemeiről fogunk koncentrálni:

- Folyamatok (amelyek a saját ellenőrzési és külső elemek)
- Kommunikációs (más néven adatfolyamok)
- A tároló (más néven adattárolók)

#### <a name="processes"></a>Folyamatok

Az egyes kategóriák az Azure IoT architektúra vázolt érdekében megpróbáljuk adat/információ megtalálható a különböző szakaszok között számos különböző veszély csökkentésére: folyamat, a kommunikáció és a tárolás. Az alábbi ad áttekintést a leggyakrabban a "folyamat" kategória, hogyan ezek sikerült legjobb szüntethető meg áttekintése követ: 

**Tartalomhamisítást lehetővé tévő (S)**: támadó kivonatot készíthet egy eszköz, szoftver vagy hardver szinten, vagy a titkosítási kulccsal védett anyagokat, és ezt követően hozzáférést más fizikai vagy virtuális eszköz azonosítója alatt a rendszer az eszköz az kulcsalap vették át. A jó ábra távirányítók, amely bármely TV kapcsolja, és eszközök népszerű prankster.

**Szolgáltatás elérhetetlenségét (D)**: az eszköz működik, vagy zavarja a rádiófrekvencia- vagy daraboló vezetékek történő kommunikációhoz képes sikerült értelmezni. Például, amely a teljesítmény vagy a hálózati kapcsolat szándékosan kiejtése felügyeleti kamera nem küld jelentést adatok egyáltalán.

**Illetéktelen (T)**: a támadó részben vagy egészben helyettesítheti az eszközön futó szoftver potenciálisan lehetővé teszi az eszköz valódi azonosságát formátumról, mintha az kulcsalap vagy a kriptográfiai kulcs anyagok tároló létesítmények rendelkezésére a tiltott program a kicserélt szoftver. A támadó például és a kommunikációs útvonalat az eszköz adatainak elrejtése és hitelesítve van, a lopott kulccsal védett anyagokat a hamis adatokkal felülírja a kibontott kulcsalap is kihasználhatja.

**(I) adatokhoz való illetéktelen hozzáférést**: Ha az eszköz úgy szoftver fut, úgy szoftverhez sikerült potenciálisan kiszivárgó adatok jogosulatlan felek részére. A támadó például előfordulhat, hogy emelés kibontott kulcsalap Fecskendezzünk be magát a kommunikációs útvonalat az eszköz és a vezérlő vagy a mező átjáró vagy felhő átjáró siphon ki információk között.

**Jogosultság illetéktelen megszerzését (E)**: olyan eszköz, amely adott funkciót is mindenképpen valami másra. Például egy szelepet, amely a megnyitásához felénél van programozva is lehet veszélye áll fenn a megnyitásához.

| **Összetevő** | **Fenyegetés** | **Kockázatcsökkentő**                                                                                                                                                | **Kockázat**                                                                                                                                                                                                    | **Végrehajtása**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eszköz        | S          | Identitás hozzárendelése az eszköz és az eszköz hitelesítése                                                                                                | Valamely eszköz cseréje eszköz vagy az eszköz része. Hogyan tudjuk meg, hogy azt a megfelelő eszközt beszélünk?                                                                                           | Hitelesíti az eszközt, a Transport Layer Security (TLS) vagy az IPSec segítségével. Infrastruktúra támogatnia kell az előmegosztott kulcs (PSK) használatával ezek az eszközök teljes aszimmetrikus titkosítás nem tudja kezelni a. Emelés Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Tamperproof mechanizmusok alkalmazásához az eszköz például oly módon, hogy nagyon nehéz lehetetlen kulcsok és más kriptográfiai anyagot kinyerni az eszköz számára. | A kockázat esetén, ha valaki az illetéktelen hozzáférés az eszköz (fizikai beavatkozás). Hogyan történik a Microsoft, ellenőrizze, hogy az eszköz nem módosította.                                                                                 | A leghatékonyabb kezelési egy platformmegbízhatósági modul (TPM) képesség, amely lehetővé teszi a kulcsok tárolása különleges-chip áramkört, amelyről a kulcsok nem lehet olvasni, de csak a kulcsot, de soha ne tegye közzé a kulcs titkosítási műveletekhez használható. Az eszköz memória-titkosítás. A legfontosabb vezetők az eszköz. A kód aláírása. |
|               | E          | Tekintettel a hozzáférés-szabályozás az eszköz. Engedélyezési rendszer.                                                                                                    | Ha az eszköz lehetővé teszi az egyes tevékenységeket, amelyek végrehajtása alapján egy külső forrásból, vagy akár feltört érzékelők parancsok, akkor lehetővé teszi a támadás műveleteket egyébként nem elérhető. | Tekintettel az engedélyezési rendszer, az eszköz                                                                                                                                                                                                                                                                                                             |
| A mező átjáró | S          | Hitelesítése az átjáró mező felhő átjáró (cert alapú, PSK, jogcím alapján,...)                                                                           | Ha valaki hamisíthatnak mező átjáró, majd azt tudja mutatni magát bármely eszköz.                                                                                                                               | TLS RSA/PSK, IPSe, [RFC 4279](https://tools.ietf.org/html/rfc4279). Mint kulcs tárolási és igazolási vonatkozik az eszközök általában – legjobb esetben van TPM használatát. Az IPSec támogatja a vezeték nélküli érzékelő hálózatok (WSN) 6LowPAN kiterjesztése.                                                                                                              |
|               | TRID       | A mező átjáró (TPM?) hamisítás elleni védelme                                                                                                            | Illetéktelen adatelérést és adatmódosítás megtévesztésére a felhő átjáró gondolkodás mező átjáróhoz folytat beszélgetést azonosítócsere eredményezheti                                                             | Memória titkosítás TPM's, hitelesítés.                                                                                                                                                                                                                                                                                                              |
|               | E          | Mező átjáró hozzáférést ellenőrző mechanizmus                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Íme néhány példa az ebbe a kategóriába tartozó veszélyek:

Azonosító adatok cseréje: A támadó kivonatot készíthet titkosítási kulccsal védett anyagokat egy eszközről, vagy a szoftver vagy hardver szinten, és ezt követően más fizikai vagy virtuális eszköz azonosítója alatt a rendszer az eszköz az kulcsalap vett hozzáférést.

**Szolgáltatás elérhetetlenségét**: az eszköz működik, vagy zavarja a rádiófrekvencia- vagy daraboló vezetékek történő kommunikációhoz képes sikerült értelmezni. Például, amely a teljesítmény vagy a hálózati kapcsolat szándékosan kiejtése felügyeleti kamera nem küld jelentést adatok egyáltalán.

**Tampering**: a támadó részben vagy egészben helyettesítheti az eszközön futó szoftver potenciálisan lehetővé teszi az eszköz valódi azonosságát formátumról, mintha az kulcsalap vagy a kriptográfiai kulcs anyagok tároló létesítmények rendelkezésére a tiltott program a kicserélt szoftver.

**Tampering**: A felügyeleti kamera, amely a látható spektrum egy üres Előszoba képe látható egy ilyen Előszoba fényképe célzó sikerült. Füst vagy a tűzvédelmi érzékelő sikerült jelez egy világosabb alatta betöltő személy. Mindkét esetben az eszköz valószínűleg műszakilag teljesen megbízható, a rendszer felé, de úgy adatokat jelzi.

**Tampering**: a támadó és a kommunikációs útvonalat az eszköz adatainak elrejtése és hitelesítve van, a lopott kulccsal védett anyagokat a hamis adatokkal felülírja a kibontott kulcsalap is kihasználhatja.

**Tampering**: a támadó teljes mértékben vagy részben helyettesítheti az eszközön futó szoftver formátumról az eszköz valódi azonosságát, mintha az kulcsalap vagy a kriptográfiai kulcs anyagok tároló létesítmények rendelkezésére a tiltott program a kicserélt szoftver lehetővé.
   
**Adatokhoz való illetéktelen hozzáférést**: Ha az eszköz úgy szoftver fut, úgy szoftverhez sikerült potenciálisan kiszivárgó adatok jogosulatlan felek részére.

**Adatokhoz való illetéktelen hozzáférés**: a támadó esetleg emelés kibontott kulcsalap Fecskendezzünk be saját maga az eszköz és a vezérlő vagy a mező átjáró vagy felhő átjáró ki információt siphon közötti kommunikációs útvonalat.

**Szolgáltatás elérhetetlenségét**: az eszköz ki van kapcsolva, vagy nincs lehetőség (vagyis a számos ipari gépek szándékos) ahol kommunikációs mód feladatra.

**Tampering**: az eszközt újra kell konfigurálni a vezérlőrendszer (kívül ismert kalibrálási paraméterek) ismeretlen állapotban működik, és így biztosítja az adatok hibásan lesznek értelmezve

**Jogosultság illetéktelen megszerzése**: olyan eszköz, amely adott funkciót is mindenképpen valami másra. Például egy szelepet, amely a megnyitásához felénél van programozva is lehet veszélye áll fenn a megnyitásához.

**Szolgáltatás elérhetetlenségét**: az eszköz nincs lehetőség ahol kommunikációs állapotba kapcsolható.

**Tampering**: az eszközt újra kell konfigurálni a vezérlőrendszer (kívül ismert kalibrálási paraméterek) ismeretlen állapotban működik, és így biztosítja az adatok hibásan lesznek értelmezve.
 
**Tartalomhamisítást lehetővé/Tampering/megtagadása**: Ha nem biztonságos (vagyis csak ritkán fogyasztói távvezérlők esetében) a támadó névtelenül kezelhető eszköz állapotát. A jó ábra távirányítók, amely bármely TV kapcsolja, és eszközök népszerű prankster.

#### <a name="communication"></a>Kommunikáció

Veszélyek eszközök, eszközök és mező átjárók és az Internetátjáró eszköz és a felhő közötti kommunikáció görbe körül. Az alábbi táblázat néhány útmutatás az eszközzel vagy virtuális Magánhálózattal nyitott szoftvercsatornáit körül van:

| **Összetevő**               | **Fenyegetés** | **Kockázatcsökkentő**                                      | **Kockázat**                                                                                                      | **Végrehajtása**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eszköz IoT Hub              | TID        | (D) TLS (PSK/RSA) a forgalom titkosításához.             | Lehallgatás vagy ütközik az eszköz és az átjáró közötti kommunikáció                             | Biztonsági protokoll szintjén. Egyéni protokollokkal kell találnia, hogy hogyan védheti meg őket. A legtöbb esetben a kommunikáció történik az eszközről a IoT hub (az eszköz kezdeményezi a kapcsolatot).                                                                                                                                                                 |
| Eszköz-eszköz               | TID        | (D) TLS (PSK/RSA) a forgalom titkosításához.            | Az eszközök közötti árutovábbítási adatok olvasásakor. Az adatok manipulálása. Az új kapcsolatok eszköz túlterhelés | Biztonsági protokoll szinten (MQTT/AMQP/HTTP/CoAP. Egyéni protokollokkal kell találnia, hogy hogyan védheti meg őket. A DoS fenyegetés enyhítő eszközök felhő vagy mező átjárón keresztül peer, azok csak törvény felé a hálózati ügyfelek is. Tekintettel az átjáró által történt brokered után a partnerek között közvetlen kapcsolat a peering eredményezhet |
| Külső entitás eszköz      | TID        | Az eszköz külső entitás erős párosítás | A kapcsolatot a tárolóeszközhöz lehallgatás. A kommunikáció zavarja az eszköz                     | Az NKK/Bluetooth LE eszköz külső entitás biztonságosan párosítás. Az eszköz (fizikai) működési panel vezérlése                                                                                                                                                                                                                                                  |
| A mező a felhő átjárók | TID        | TLS (PSK/RSA) a forgalom titkosításához.               | Lehallgatás vagy ütközik az eszköz és az átjáró közötti kommunikáció                             | Biztonsági protokoll szinten (MQTT/AMQP/HTTP/CoAP). Egyéni protokollokkal kell találnia, hogy hogyan védheti meg őket.                                                                                                                                                                                                                                                       |
| Felhő Internetátjáró eszköz        | TID        | TLS (PSK/RSA) a forgalom titkosításához.               | Lehallgatás vagy ütközik az eszköz és az átjáró közötti kommunikáció                             | Biztonsági protokoll szinten (MQTT/AMQP/HTTP/CoAP). Egyéni protokollokkal kell találnia, hogy hogyan védheti meg őket.                                                                                                                                                                                                                                                       |

Íme néhány példa az ebbe a kategóriába tartozó veszélyek:

**Szolgáltatás elérhetetlenségét**: korlátozott eszközök általában alatt álló DoS amikor azok aktívan figyelni a bejövő kapcsolatok vagy kéretlen datagram a hálózaton, mert a támadó megnyithatja sok kapcsolat párhuzamosan és nem szolgáltatás azokat vagy szolgáltatás őket nagyon lassan, vagy elárasztott kéretlen forgalmat is lehet. Mindkét esetben az eszköz is hatékonyan lehet ezeket működésre alkalmatlanná tették a hálózaton.

**Beépülő navigációs címsávkijátszási, adatokhoz való illetéktelen hozzáférést**: korlátozott eszközök és speciális eszközök gyakran rendelkeznek egy összes biztonsági létesítmények, például a jelszót vagy PIN-védelem, vagy teljes mértékben támaszkodnak a hálózat számára biztosított információkhoz való hozzáférés egy eszközt a hálózaton van, és gyakran csak védett hálózat megosztott kulcs tehát megbízható. Amely azt jelenti, hogy eszköz vagy a hálózaton a megosztott titkot kell közzétenni, ha az eszközt vagy az eszköz által kibocsátott adatok megfigyeléséhez lehetséges.  

**Tartalomhamisítást lehetővé**: a támadó is elfoghatják vagy részben felülírják az adás és meghamisíthatja a megbízó (a középső ember)

**Tampering**: a támadó is elfoghatják vagy részben felülírják az adás és hamis információt küld a 

**Adatokhoz való illetéktelen hozzáférést:** a támadó kihallgassa a közvetítés és megkaphatják az információk engedély nélküli **szolgáltatásmegtagadást:** a támadó előfordulhat, hogy a szórt jel dzsem és telepítési információ megtagadása

#### <a name="storage"></a>Tárolás

Minden eszköz és a mező átjáró tároló (az adatok, az operációs rendszer lemezkép tárolási Queuing ideiglenes) rendelkezik.

| **Összetevő**                            | **Fenyegetés** | **Kockázatcsökkentő**                       | **Kockázat**                                                                                                                                                                                                                                                                                                                | **Végrehajtása**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tároló eszköz                           | TRID       | Tárolási titkosítás, a naplók aláírása | Távmérő adatok manipulálása a tárolóból (Gyűjtenek adatokat), adatok olvasása. Meghamisítása várólistára vagy a gyorsítótárazott adatok parancsot. Konfigurációs vagy belső vezérlőprogram frissítőcsomagok manipulálása gyorsítótárazott vagy helyileg aszinkron vezethet feltörési OS és/vagy rendszer-összetevők                                         | Titkosítás, az üzenet hitelesítési kód (MAC) és a digitális aláírás. Ha lehetséges, erős hozzáférés-erőforrás-hozzáférés vezérlő listák (ACL) vagy az engedélyek. |
| Az eszköz operációs rendszere kép                          | TRID       |                                      | OS manipulálása / cseréje az operációs rendszer összetevőit.                                                                                                                                                                                                                                                                         | Csak olvasható operációs rendszer partíciójára, aláírt OS kép, titkosítás                                                                                                                    |
| (Az adatok queuing) átjáró tároló mező | TRID       | Tárolási titkosítás, a naplók aláírása | Távmérő adatok manipulálása a tárolóból (Gyűjtenek adatokat), adatok olvasása meghamisítása várólistára vagy gyorsítótárazott adatok parancsot. Konfigurációs vagy belső vezérlőprogram frissítési csomagok (szánt eszközök vagy mező átjáró) manipulálása gyorsítótárazott vagy helyileg aszinkron vezethet feltörési OS és/vagy rendszer-összetevők | A BitLocker                                                                                                                                                              |
| Mező átjáró-OS kép                   | TRID       |                                      | OS manipulálása / cseréje az operációs rendszer összetevőit.                                                                                                                                                                                                                                                                          | Csak olvasható operációs rendszer partíciójára, aláírt OS kép, titkosítás                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Eszköz és az esemény feldolgozás/felhő átjáró zóna

Felhő az átjáró az rendszer, amely lehetővé teszi a távoli kommunikációt eszközök vagy mező átjárók különböző helyekről és keresztül nyilvános hálózati helyre, általában egy felhőalapú ellenőrzési és elemző rendszer, az ilyen rendszerek egy összevonási felé. Bizonyos esetekben felhő átjáró azonnal megkönnyíthetik access speciális eszközöket a terminálok tabletták vagy telefonokhoz. Az itt tárgyalt összefüggésében "felhő" hivatott hivatkozik a dedikált adatfeldolgozó rendszer ugyanazon a helyen, mint a csatolt eszközökről vagy mező átjárók nem kötött, és ahol operatív intézkedések elkerülése célzott fizikai hozzáférést, de nem szükségszerűen egy "nyilvános felhő" infrastruktúrához.  Felhő átjáró potenciálisan társítja a virtualizálás hálózati átfedés és az összes csatlakoztatott eszközök vagy a mező átjárók a hálózati forgalmat a felhő átjáró háromszintű. A felhő átjáró, önmagában egy sem eszköz ellenőrző rendszer, sem a feldolgozó létesítmény vagy tárolóhely eszköz adatok; Ezekben a létesítményekben felület felhő átjáró. A felhő átjáró a felhő átjáró maga mező átjárók és a kapcsolódó gyermekeszközt közvetlenül vagy közvetetten tartozik.

Felhő átjáró az leginkább kitett végpontok, amelyre mező átjáró és az eszköz csatlakozik a szolgáltatásként futó szoftver beépített egyéni darab. Ilyen úgy kell megtervezni, biztonsági szem előtt. Kövesse megtervezése és elkészítése a szolgáltatás folyamata [SDL](http://www.microsoft.com/sdl) . 

#### <a name="services-zone"></a>Szolgáltatások zóna

Ellenőrzési rendszer (vagy tartományvezérlő), hogy egy eszköz vagy mező átjáró vagy felhő átjáró egy vagy több eszköz ellenőrzésére és/vagy gyűjtése és/vagy tárolására és/vagy eszköz adatelemzés bemutató, vagy utólagos ellenőrzésének céljából felületek szoftveres megoldást. Ellenőrzési rendszereket, amelyek közvetlenül elősegítik a személyek interakció vitafórumban hatálya alá csak entitások. A kivétel olyan közbenső fizikai ellenőrzés felületek eszközökön, például egy kapcsoló, amely lehetővé teszi az eszköz kikapcsolása vagy más tulajdonságainak módosítása olyan személy, és amely nincs digitálisan elérhető funkcionális megfelelője. 

Köztes fizikai ellenőrzés felületek azok ahol tetszőleges logikai irányadó korlátozza a függvény a fizikai ellenőrzés felület, hogy távolról egyenértékű függvénnyel kell kezdeményezni vagy távoli input bemeneti ütközik el kell kerülni – ilyen intermediated vezérlő felületek adatbázistáblákhoz vannak csatolva a helyi vezérlőrendszer ötvözi az alapul szolgáló funkcióját is más távvezérlő rendszer, amely az eszköz párhuzamosan kell csatolni. Felső fenyegető veszélyek a felhő számítástechnikai [Felhő Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) lapon olvasható.


## <a name="additional-resources"></a>További források

További információt a következő cikkekben talál:

- [SDL fenyegetés modellezési eszköz](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [A Microsoft Azure IoT referencia-architektúra](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
