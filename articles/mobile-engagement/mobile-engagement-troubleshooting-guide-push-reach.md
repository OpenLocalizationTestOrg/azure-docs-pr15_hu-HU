<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet hibaelhárítási útmutatójának - leküldéses/vannak" 
   description="Felhasználói beavatkozás és értesítés problémáinak megoldása az Azure Mobile tetszés szerint elmélyedhet" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Útmutató a hibaelhárítás leküldéses és elérje a problémák

Az alábbi táblázat a lehetséges problémákat, hogyan Azure Mobile tetszés szerint elmélyedhet adatokat küld a felhasználóknak a előfordulhatnak.
 
## <a name="push-failures"></a>Leküldéses hibák

### <a name="issue"></a>A probléma
- Veremmutatót (kívül alkalmazásban, illetve mindkettő alkalmazásban) nem működnek.

### <a name="causes"></a>Okok
- Sokszor leküldéses hiba jelzi, hogy Azure Mobile tetszés szerint elmélyedhet, vannak vagy más speciális szolgáltatás Azure Mobile tetszés szerint elmélyedhet a nem megfelelően integrált vagy, hogy egy frissítés szükséges a SDK ismert probléma megoldásához és új operációs rendszer vagy más eszközhöz platform.
- Tesztelje a csak az alkalmazás a leküldéses, és csak egy "Házon kívül" alkalmazás leküldéses annak megállapításához, hogy egy üzenetet az alkalmazást vagy a "Házon kívül" alkalmazás problémát.
- Tesztelje a felhasználói felület-és az API megtudhatja, milyen további információ a hibáról elérhető hibaelhárítási lépésként elláthatja mindkét helyről.
- Ki letölthető veremmutatót nem fognak működni, kivéve, ha a SDK integrált Azure Mobile tetszés szerint elmélyedhet és a gyermekektől.
- Veremmutatót nem fog működni, ha a tanúsítványok nem érvényes, vagy hibásan használja TERMÉKKATALÓGUS a fejlesztői és összehasonlítása (csak iOS esetén). (**Megjegyzés:** "ki letölthető" leküldéses értesítései nem terjeszthetők IOS-en, ha mindkét fejlesztését (Fejlesztők), és az alkalmazás telepítve ugyanarra az eszközre, mivel a biztonsági jogkivonat társított a tanúsítvány üzemi (TERMÉKKATALÓGUS) verzióinak előfordulhat, hogy érvényét, Apple által. A probléma megoldásához távolítsa el az alkalmazás Fejlesztők és a gyártási verziója, majd telepítse újra az eszközön csak az egyik verzióból.)
- Alkalmazás leküldéses kívül megszámolja történő eltérően más platformokon (iOS Android-nál kevesebb információkat jelenít meg, ha natív veremmutatót le vannak tiltva eszközön, az API a felhasználói felület több információt tartalmaznak a leküldéses stat).
- Ki letölthető veremmutatót blokkolhatja ügyfelek (iOS és Android) operációs rendszer szintjén.
- Ki letölthető veremmutatót fog megjelenni tiltva az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület, ha helyesen nem integrált, de az API-t a háttérben meghiúsulhat.
- Az alkalmazás veremmutatót nem fognak működni, kivéve, ha a SDK integrált Azure Mobile tetszés szerint elmélyedhet és a gyermekektől.
- GCM és ADM veremmutatót nem fognak működni, kivéve, ha az Azure Mobile tetszés szerint elmélyedhet és az adott kiszolgáló integrált a SDK (csak Android).
- Az alkalmazás és a "Házon kívül" alkalmazás veremmutatót kell vizsgálni külön-külön állapítsa meg, hogy a leküldéses vagy vannak probléma.
- Veremmutatót megkövetelése, hogy az alkalmazás csak a megnyitott kapott alkalmazásban.
- Az alkalmazás veremmutatót mérete gyakran beállítás egy funkcióról vagy lemondási alkalmazás információ címke szerint szűrhető.
- Ha egyéni kategória használni az alkalmazást az értesítések megjelenítését vannak, akkor hajtsa végre a megfelelő életciklus-az értesítés, különben az értesítés nem törlődik amikor a felhasználó mellőzheti.
- Ha nincs záró dátum egy marketingkampány kezdődik, és egy eszközt a az alkalmazás kap értesítést, de nem jelenik meg az értesítés a következő alkalommal jelentkezik be az alkalmazásba, azt még, továbbra is a felhasználó fog kapni, akkor is, ha saját kezűleg befejezheti a marketingkampány-hatással van.
- A problémák a leküldéses API-val győződjön meg arról, hogy valóban használni kívánt a leküldéses API helyett a elérjen API (mivel a elérjen API gyakran használják, hogy több), hogy a "tartalom" és "bejelentő" paraméterek nem zavaró vannak.
- Tesztelje a leküldéses marketingkampány mindkét WiFi-csatlakozás és a 3G kattintva kiküszöbölheti a hálózati kapcsolat problémákat lehetséges adatforrásként keresztül csatlakoztatott eszközön.

## <a name="push-testing"></a>Leküldéses tesztelése

### <a name="issue"></a>A probléma
- Veremmutatót lehet küldeni egy adott eszköz alapján egy eszköz azonosítót.

### <a name="causes"></a>Okok

- Vizsgálati eszközöket platformokon másképp beállítása, de a Eszközazonosítót található összes platformokon kell működniük esemény okoz próba eszközön az alkalmazást, és a Eszközazonosítót a portálon keres.
- Vizsgálati eszközöket másképp működnek az összehasonlítás IDFV IDFA (csak iOS esetén).


## <a name="push-customization"></a>Leküldéses testreszabása

### <a name="issue"></a>A probléma
- Speciális elem nem működése leküldéses (belépőkártya, gyűrű, rezgés, kép, stb.).
- Veremmutatót hivatkozásainak (Házon kívül az alkalmazás-alkalmazásban, a webhelyre, egy helyre alkalmazásban) nem működnek.
- Leküldéses statisztikai adatokat mutatják, hogy a leküldéses nem küldött várható szerinti számú személyt (túl sok vagy nincs elég).
- Leküldéses létrejön egy újabb példánya, és a kapott kétszer.
- Nem tudja regisztrálni próba eszköz az Azure Mobile tetszés szerint elmélyedhet verembe küldi (alkalmazással saját termékkatalógus vagy a Fejlesztők).

### <a name="causes"></a>Okok

- Az alkalmazás egy adott helyére mutató hivatkozás "kategória" (csak Android) szükséges.
- Felhasználók átirányítása egy másik helyre a leküldéses értesítést parancsra kattintást követően mély össze rendszerek kell a létrehozott, és az alkalmazás és a eszköz operációs rendszere nem Mobile tetszés szerint elmélyedhet a közvetlenül kezeli. (**Megjegyzés:** ki letölthető értesítések nem csatolhat közvetlenül az iOS alkalmazás helyeken csak Android tudja.)
- Külső kép kiszolgálók kell használni a HTTP "GET" és "Központi" a folyamat áttekintése látható verembe küldi (csak Android) használata.
- A kód az Azure Mobile tetszés szerint elmélyedhet ügynök letiltása a rendszer, amikor a billentyűzet, és a kód újra aktiválja az Azure Mobile tetszés szerint elmélyedhet ügynök, után a billentyűzet nincs megnyitva, hogy a billentyűzet nem jelennek meg a az értesítésben (csak iOS esetén).
- Egyes elemeket nem működnek a próba szimulációk, de csak valós kampányok (belépőkártya, gyűrű, rezgés, kép, stb.).
- Nem egymás kiszolgálóadatok be van jelentkezve, "tesztelése" veremmutatót használatakor a gombra. Adatok valós leküldéses kampányok csak be van jelentkezve.
- Szeretné azonosítani a problémát, elhárítása: a vizsgálat, hasonlóan, és a valós marketingkampány, mivel ezek a kissé másképp működnek.
- A "alkalmazásban" és "bármikor" kampányok mikorra van ütemezve idejének hosszát is hatással kézbesítési számok, mivel a marketingkampány csak kerülnek a felhasználók, akik szerepelnek "alkalmazás" a marketingkampány futása közben (és a felhasználók, akik rendelkeznek a "ki letölthető" Ha értesítést szeretne kapni az eszköz beállításainak).
- Hogyan Android és iOS kezelik kívül alkalmazásértesítések közötti különbségek nehezebben közvetlenül a leküldéses statisztika között az alkalmazás Android és iOS verziójának összehasonlítása. Android-alapú további információk OS szintű értesítést, mint az iOS-e. Android-alapú jelentések natív értesítést kapott, kattintott vagy az értesítési központban törölt, de iOS nem jelentés ezt az információt, kivéve ha az értesítés kattintott. 
- A fő oka, hogy a számok "tolt" eltérnek, mint más, mint az "i" számok elérjen kampányok, hogy "alkalmazásban" és "ki letölthető" értesítések számítanak másképp. "Alkalmazásban" értesítések Mobile tetszés szerint elmélyedhet kezeli, de a "ki letölthető" értesítést a rendszer az eszköz értesítési projektközpontjában kezeli.

## <a name="push-targeting"></a>Leküldéses célba juttatása

### <a name="issue"></a>A probléma
- Beépített célba juttatása nem a várt módon működik.
- Alkalmazás információ címke kiválasztásával nem működik megfelelően.
- GEO helyfüggő célba juttatása nem működik megfelelően.
- Nyelvi beállítások nem működnek a várt módon működik.

### <a name="causes"></a>Okok

- Győződjön meg arról, hogy feltöltött alkalmazás információ címkék az Azure Mobile tetszés szerint elmélyedhet felhasználói felület vagy a API-val.
- A leküldéses sebességét vagy az alkalmazás szintjén leküldéses kvóta szabályozásának vagy korlátozása a közönség marketingkampány szintre megakadályozhatja, hogy egy személy egy adott leküldéses fogadása, még akkor is, ha a többi célkritériumokat megfelelnek. 
- "Nyelv" beállítása különbözik célba juttatása alapján ország vagy területi, amely szintén nem célba juttatása Geo-helye alapján a telefon helyét vagy a GPS helyét.
- Az "alapértelmezett nyelvű" üzenet érkezik bármely felhasználói, akik nem rendelkeznek az eszköze, a másodlagos nyelvek adja meg.


## <a name="push-scheduling"></a>Leküldéses ütemezése

### <a name="issue"></a>A probléma
- Leküldéses ütemezési nem a várt módon működik (küldése túl korai vagy késleltetett).

### <a name="causes"></a>Okok

- Időzóna is az ütemezés, problémák, különösen akkor, ha a végfelhasználók időzóna használatával.
- Speciális leküldéses szolgáltatások késleltetheti veremmutatót.
- Mivel az Azure Mobile tetszés szerint elmélyedhet adatok leküldéses elküldése előtt mindig kérése a telefon valós időben lehet célba juttatása, a beállítások (helyett az alkalmazás információ címkék) késleltetheti telefonszám alapján verembe küldi.
- A záró dátum nélküli létrehozott kampányok a leküldéses helyben tárolni az eszközre, és a megjelenítése, akkor a következő alkalommal, amikor az alkalmazás nyitja meg, ha a marketingkampány manuálisan véget ér.
- Egynél több marketingkampány indítása egyszerre szeretne képet beolvasni a felhasználó alap (csak kezdődik egy marketingkampány egyszerre legfeljebb négy, is csak az aktív felhasználók a cél, hogy a régi felhasználók nincs beolvasandó próbálja) hosszabb időt vehet igénybe.
- Ha a "Figyelmen kívül hagyása célközönséget, leküldéses küld az API-t a felhasználók" beállítás a gyermekektől marketingkampány "Marketingkampány" szakaszában a marketingkampány automatikusan nem küld, szüksége lesz küldje el manuálisan a elérjen API keresztül.
- Ha egyéni kategória használni az alkalmazást az értesítések megjelenítését vannak, akkor hajtsa végre a megfelelő életciklus-értesítés, különben az értesítés nem törlődik amikor a felhasználó mellőzheti.

 
