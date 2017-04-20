# <a name="internet-of-things-security-best-practices"></a>Az Internet a dolog biztonsággal kapcsolatos gyakorlati tanácsok

Egy dolog az Internet (IoT) biztonságos infrastruktúra szigorú biztonsági jellegű stratégiát igényel. Ezt a stratégiát igényel a felhőben adatvédelem, az árutovábbítási adatok integritásának védelme a nyilvános interneten keresztül, és biztonságosan kiépítése az eszközök. Minden réteg épít a teljes infrastruktúra nagyobb biztonsági garanciát.

## <a name="secure-an-iot-infrastructure"></a>IoT infrastruktúra biztonságos

A biztonsági jellegű stratégia is fejlett, és a gyártási, fejlesztési és telepítési IoT eszközök és az infrastruktúra érintett különböző szereplők aktív részvételével végrehajtott. Következő ezeket részletes leírását is.  

- **IoT hardver gyártó/integrátor**: ezek általában IoT hardver üzembe kiegészítők hardver összeszerelés különböző gyártók vagy szállítók hardver nyújtó gyártott vagy más szállítók által integrált IoT-telepítéshez gyártói.
- **IoT megoldás fejlesztője**: egy IoT-megoldás fejlesztése szokás szerint a megoldás fejlesztőjének. A fejlesztői része lehet egy házon belüli csoport vagy a tevékenység szakosodott rendszer integrátor (SI). A IoT megoldás fejlesztője IoT oldat teljesen különböző összetevők kidolgozása, vásárolt vagy nyílt forrásból különböző összetevők integrálására, vagy fogadja el az előre konfigurált megoldások kisebb kiigazítás.
- **IoT-oldat deployer**: an IoT után fejlesztik, mezőjében telepítendő szükséges. Ez magában foglalja a telepítési hardver eszközök összekapcsolása, és a telepítési megoldások a hardvereszközök vagy a felhő.
- **IoT megoldás operátor**: után IoT megoldás telepítve van, a szükséges hosszú távú műveletek figyelemmel kísérése, frissítése és karbantartása. Ez egy házon belüli csoport, amely magában foglalja az információk informatikai szakemberek, hardverelemek működésének és karbantartási csoportok és a tartomány szakemberek, aki teljes IoT-infrastruktúra megfelelő működésének figyelemmel végzi.

Az alábbi részekben fejlesztése, telepítése és egy biztonságos IoT infrastruktúrát működtetni e szereplők minden gyakorlati tanácsokat.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT hardver gyártó/integrátor

Az alábbi táblázat a gyakorlati tanácsok a hardvergyártók IoT és hardver kiegészítők.

- **Hatókör hardver minimális követelmények**: A hardver Tervező tartalmaznia kell a hardvert, és semmi további működéséhez szükséges minimális szolgáltatások. Ennek egyik példája az USB port is, csak akkor, ha az eszköz működéséhez szükséges. Ezek a szolgáltatások nyissa meg az eszközt el kell kerülni a szükségtelen támadási.
- **Ellenőrizze a hardver bizonyítékot átállítani**: mechanizmusok észleli a fizikai manipulálása, például az eszköz fedőlap megnyitása vagy egy részét az eszköz eltávolítása a Build. Ezek a jelek átállítani a felhő, amelyek figyelmeztetik a piaci szereplők események sikerült feltölteni a adatfolyam részét képezheti.
- **Biztonságos hardver körül build**: lehetővé teszi, ha ELÁBÉ, biztonsági szolgáltatások, például a biztonságos és titkosított tároló létrehozásához vagy rendszerindító a platformmegbízhatósági modul (TPM) alapú szolgáltatásokat. Ezek a szolgáltatások ellenőrizze eszközök több biztonságos, és a teljes IoT infrastruktúra védelme.
- **Ellenőrizze a biztonságos frissítések**: az eszköz élettartama során a belső vezérlőprogram-frissítések elkerülhetetlen. Frissítések és a belső vezérlőprogram verziója kriptográfiai minőségbiztosítási eszközök biztonságos görbék létrehozása lehetővé teszi az eszköz biztonságos legyen, alatt és után a frissítést.

## <a name="iot-solution-developer"></a>IoT megoldás fejlesztője

IoT megoldásfejlesztők számára a gyakorlati tanácsok a következők:

- **Kövesse a szoftver biztonságos fejlesztési módszertan**: biztonságos szoftverfejlesztés a kezdetektől egészen a projekt annak végrehajtását, a vizsgálat és a központi telepítés, biztonsági föld-up erőfeszítés szükséges. A választási lehetőségek platformok, nyelvek és eszközök összes befolyásolja, ezt a módszert. A Microsoft biztonsági fejlesztés életciklus építési biztonságos szoftver közelítést biztosít.
- **Válassza ki a Megnyitás-forrás szoftver gonddal**: Open-source szoftver lehetőséget nyújt egy gyors megoldások fejlesztése céljából. Megnyitás-adatforrás kiválasztásakor is érdemes aktivitási szintjét a Közösség minden open-source összetevőt. Egy aktív Közösség biztosítja, hogy a szoftver támogatott, és hogy problémák felderítése és címzett. Másik lehetőségként előfordulhat egy takarják el és inaktív open-source szoftver nem támogatja, és problémák valószínűleg nem észlelhetők.
- **A gondozás**: könyvtárak és API-k határán létezik számos szoftver biztonsági hiányosságokat. Funkció, amely nem feltétlenül szükséges a jelenlegi telepítési még az API réteg keresztül elérhető lehet. Teljes biztonsága érdekében ellenőrizze, hogy valamennyi összeköttetés alkatrészek mindegyike integrált biztonsági hibákat.      

## <a name="iot-solution-deployer"></a>IoT-oldat deployer

IoT-oldat deployers kapcsolatos gyakorlati tanácsok a következők:

- **Hardver biztonságos telepítése**: IoT telepítések nem biztonságos helyen, például nyilvános szóközt vagy felügyelet nélküli területi telepítendő hardver lehet szükség. Ilyen esetekben biztosítani, hogy hardver telepítési hamisíthatatlan büntetést. USB vagy más portokat a hardver érhetők el, ha biztosítja a biztonságos szabályozott. Sok támadási ezek belépési pontként használható.
- **Hitelesítési kulcsok biztonsága**: a telepítés során minden egyes eszköz igényel az eszköz azonosítóját és a kapcsolódó hitelesítési kulcsokat a felhőalapú szolgáltatás által létrehozott. Ezeket a kulcsokat fizikailag biztonságos tartani még a telepítés után. Feltört kulcs segítségével rosszindulatú eszköz álcázzák magukat, a létező eszközöket.

## <a name="iot-solution-operator"></a>IoT megoldás operátor

IoT megoldás piaci szereplők számára a gyakorlati tanácsok a következők:

- **A rendszer naprakész állapotban tartása**: Győződjön meg arról, hogy eszköz operációs rendszerek és az összes eszköz-illesztőprogramot frissíteni a legújabb verzióra. Ha bekapcsolja az automatikus frissítések Windows 10 (IoT vagy más SKU), a Microsoft aktualizálja azt, a biztonságos operációs rendszer IoT eszközök biztosítása. Más operációs rendszerekhez (például Linux) vezetése naprakészen segít biztosítani, hogy is védve vannak rosszindulatú támadások ellen.
- **Rosszindulatú tevékenység elleni védelme**: Ha az operációs rendszer lehetővé teszi, a legújabb víruskereső és antimalware képességek telepítése minden eszköz operációs rendszeren. Ez segíthet a legtöbb külső fenyegetések csökkentésére. A legtöbb modern operációs rendszerek fenyegetésektől védekezésként megfelelő lépések megtétele.
- **Gyakran ellenőrzés**: IoT naplózás infrastruktúrát a biztonsággal kapcsolatos problémák esetén kulcs válaszol a biztonsági események. A legtöbb operációs rendszer beépített eseménynaplózás gyakran felül kell vizsgálni, ellenőrizze, hogy nincs biztonsági jogsértés történt meg. Naplózási adatok küldhetők külön távmérő adatfolyamként a felhőalapú szolgáltatás ahol elemezhetők.
- **Fizikai védelme a IoT infrastruktúra**: A legrosszabb támadások elleni IoT infrastruktúra eszközök fizikai hozzáférés indította el. Egy fontos biztonsági ajánlott rosszindulatú használatát, az USB port és más fizikai hozzáférés elleni védelme érdekében. Uncovering megsértésével történt lehet, hogy egy kulcs bejelentkezik a fizikai hozzáféréssel, például az USB port használatát. Újra Windows 10 (IoT és egyéb SKU) lehetővé teszi, hogy ezek az események részletes naplózása.
- **Védelme felhő hitelesítő adatok**: Felhő konfigurálásával és működtetésével IoT telepítés használt hitelesítő esetleg hozzáférhet és egy IoT behatolhatnak a legegyszerűbb módja. A jelszó gyakori megváltoztatásával a hitelesítő adatok védelmét, és tartózkodjon ezek a hitelesítő adatok használata a nyilvános számítógépeken.

Különböző IoT eszközök lehetőségei eltérőek lehetnek. Lehet, hogy egyes eszközök közös asztali operációs rendszert futtató számítógépek, és előfordulhat, hogy a bizonyos eszközök nagyon passzív operációs rendszereket futtató. A biztonsággal kapcsolatos gyakorlati tanácsok leírt korábban ezeket az eszközöket különböző mértékben alkalmazni lehet. Amennyiben, további biztonságot és gyakorlati tanácsok a gyártók ezen eszközök kell követni.

Egyes régebbi és a korlátozott eszközök lehet, hogy nem készült kifejezetten IoT telepítéshez. Ezeket az eszközöket nem lehet, hogy az adatok titkosítása, csatlakozás az internethez, vagy adja meg speciális naplózás lehetősége. Ezekben az esetekben a modern és biztonságos mező átjáró összesített adatokat a régebbi eszközök, és ezek az eszközök az interneten keresztül kapcsolódó szükséges biztonságot. Mező átjárók biztosít biztonságos hitelesítési, titkosított munkamenetek tárgyalása, a felhő, és számos egyéb biztonsági szolgáltatások parancsai kézhezvételét.
