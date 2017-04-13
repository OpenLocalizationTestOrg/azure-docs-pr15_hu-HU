<properties
    pageTitle="A tevékenységek előkészíthetik az adatokat, a továbbfejlesztett gép tanulási |} Microsoft Azure"
    description="Előre folyamat, és felkészülés azt gépi tanulási adatok letisztázásának."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>A tevékenységek előkészíthetik az adatokat, a továbbfejlesztett számítógép-oktatás

Előfeldolgozása és adatainak törlése a fontos feladatok, amely általában kell elvégezni, mielőtt adatkészlet használható hatékony gépi tanulási. Nyers adatokból gyakran zajos és mellékletei nem megbízható, és értékek hiányozhat. Ezek az adatok használatának modellezési is eredményt félrevezető. Az alábbi műveleteket a csapat adatok tudományos folyamat (TDSP) tartalma, és a szokásos kövesse az használva Fedezze fel, és az előzetes kezelés kötelező megtervezése adatkészletet, egy kezdeti feltárása. Részletesebb a TDSP folyamat, tanulmányozza a a [Csapat adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)témakörben ismertetett lépéseket.

Előzetes feldolgozás és a feladatokat, például az adatok feltárása feladat törlése a környezetben, például SQL vagy struktúra vagy Azure gépi tanulási Studio és a különféle eszközök és nyelvek, például R vagy Python, attól függően, hogy az adatok tárolásának helye és milyen formázása számos különböző is elvégezni. Mivel a TDSP közelítéses jellegű, az alábbi műveleteket történhet a különböző lépéseket az munkafolyamatokban a folyamat.

Ez a cikk bemutatja a különböző adatfeldolgozás fogalmak és feladatokhoz, amelyekhez előtt vagy után adatok ingesting az Azure gépi tanulási végezhető.

Példa adatok feltárása és Azure gépi tanulási studio belül kész előtti feldolgozása láthatja a [előfeldolgozása Azure gépi tanulási Studio adatok](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) .


## <a name="why-pre-process-and-clean-data"></a>Miért előre folyamat, és adatok letisztázásának?

Valós adatokat gyűjt különböző forrásokból származó, és a folyamatok, és szabálytalanság, illetve hogy az adatkészlet sérült adatokat tartalmazhat. A szokásos adatok minőség problémák merülnek fel a következők:

* **Hiányos**: adatok attribútumok vagy hiányzó értékeket tartalmazó hiányzik.
* **Lármás**: adatok hibás bejegyzések vagy kiugró tartalmaznak.
* **Következetlen**: adatok nem ütközik-e a bejegyzéseket, vagy eltéréseket tartalmaznak.

Minőségű adatok rendszer prediktív modellek minőségét. "Szemét szemét meg a" elkerülése és adatok minőségének, és ezért a teljesítmény modell, elengedhetetlen vagy adatok állapota képernyőn korai ismerni az adatokkal kapcsolatos problémák és döntse el, a megfelelő adatfeldolgozás és gyorsítótárának lépések elvégzéséhez.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Mik azok a néhány tipikus adatok állapota képernyőn megjelenő utasításokat a dolgozik?

Azt is ellenőrizheti az általános adatok minőségének, jelölje be a:

* A **rekordok**számát.
* **Attribútumok** (vagy a **szolgáltatások**) száma.
* Az attribútum **adattípusok** (névleges, sorszám vagy folyamatos).
* **Hiányzó értékek**számát.
* **Jól-formedness** az adatokat.
    * Ha az adatok TSV- vagy CSV, ellenőrizze, hogy a elválasztójelek oszlop és sor elválasztójelek mindig megfelelően külön oszlopok és sorok.
    * Ha az adatok HTML vagy XML formátumban, ellenőrizze, hogy az adatok jól formázott alapján meg a megfelelő szabványok.
    * Elemzési is szükség lehet a strukturált adatok kinyerése félig strukturált és strukturálatlan adatokat abból.
* **Nem következetes adatrekordok**. Jelölje be az értékek engedélyezettek. például ha az adatokat tartalmaz a diák Átlageredmény, jelölőnégyzetet, ha a kijelölt tartományban, akkor a Átlageredmény mondja ki a 0 ~ 4.

Amikor megtalálta a problémák adataival, **feldolgozás lépéseinek** szükségesek amelyek gyakran magában foglalja hiányzó értékek, adatok normalizálás, discretization, távolítsa el vagy cserélje le a beágyazott karakterek hatással lehetnek a adatok igazításának feldolgozása szöveg tisztítására, vegyes adattípusokat közös mezőket, és másokkal.

**Azure gépi tanulási szabályos táblázatos adatok fogyasztása**.  Ha adatai már táblázatos formátumban, közvetlenül az Azure gépi tanulási a gépi tanulási Studio előtti adatfeldolgozás hajtható végre.  Ha adatokat nem táblázatos formában Ismerkedés a XML-elemzési is szükség lehet annak érdekében, hogy az adatok átalakítása táblázatos formában.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Melyek a főbb tevékenységek előtti adatfeldolgozás?

* **Adatok törlése**: Töltse ki a vagy hiányzó értékek, észleli és zajos kiugró eltávolítani.
* **Átalakítási**: csökkentheti a dimenziókat és zajt adatok normalizálása.
* **Adatok csökkentésére**: minta adatrekordok vagy attribútumokat az egyszerűbb adatkezelési.
* **Adatok discretization**: folytonos attribútumok átalakítása kockák attribútumok az egyszerű használat érdekében az egyes gépi tanulási módszerek.
* **Tisztítására szöveg**: távolítsa el a beágyazott karaktereket, emiatt a adatok átrendeződését, például a beágyazott lapok tabulátorral tagolt adatfájlt, a beépített új vonalak, amelyek megszakíthatják rekordok stb.

Az alábbi szakaszok részletesen néhány adatfeldolgozás lépéseket.

## <a name="how-to-deal-with-missing-values"></a>Hogyan lehet kezelni az hiányzó értékeket?

Hiányzó értékek foglalkozik, célszerű először a probléma azonosításához jobb fogópont a hiányzó értékeket az az oka. Tipikus hiányzó érték kezelése módszereket használhatja:

* **Törlés**: hiányzó értékeket tartalmazó rekordok eltávolítása
* **Próbabábutest helyettesítés**: hiányzó értékeket cserélje ki egy üres érték: például azt, _Ismeretlen_ kockák vagy 0 numerikus értékek.
* **Helyettesítő jelenti**: Ha a hiányzó adatok nem numerikus, a hiányzó értékek helyettesítése a középérték.
* **Gyakori helyettesítés**: Ha a hiányzó adatok kockák, cserélje ki a hiányzó értékeket a leggyakoribb elem
* **Regressziós helyettesítés**: egy regresszió módszert a hiányzó értékek lecserélése regressed értékeket.  

## <a name="how-to-normalize-data"></a>Hogyan lehet az adatok normalizálása?

Adatok normalizálás újra méretezze át a megadott tartomány numerikus értékeket. Népszerű adatok normalizálás módszerek a következők:

* **A maximális min normalizálás**: lineárisan Átalakítás tartománnyá az adatokat, mondja ki a 0 és 1, ahol átméretezi a minimális érték 0 és a maximális érték 1 között.
* **Z-mutatója normalizálás**: adatok alapján értéknél és szórásnál: az adatok és a közép közötti különbség osztása a szórást.
* **Decimális méretezés**: az adatok méretezni a tizedesvesszőtől attribútumérték áthelyezésével.  

## <a name="how-to-discretize-data"></a>Hogyan lehet discretize adatok?

Adatok folytonos értékek alakításával névleges attribútumok és intervallumok discretized is lehet. Néhány módszert, ezzel a következők:

* **Egyenlő szélességű Binning**: az összes lehetséges értéke közül attribútum köre osztása azonos méretű N csoportokba, és hozzárendelése egy mappában lévő eső értékek és a bin szám.
* **Egyenlő magasságú Binning**: osztása összes lehetséges értéke közül attribútum cellatartomány N csoportokba egyes példányok ugyanannyi tartalmazó, majd hozzárendelése egy mappában lévő eső értékek és a bin szám.  

## <a name="how-to-reduce-data"></a>Hogyan csökkentheti az adatok?

Különböző módon könnyebben adatainak kezelése az adatok méretének csökkentése érdekében. Attól függően, hogy az adatok méretét és a tartomány az alábbi módszerek alkalmazhatók:

* **Rekord mintavételnél**: minta adatrekordok és csak a képviselő Alkészlet választhat az adatokat.
* **Attribútum mintavételnél**: válassza ki az egyik legfontosabb attribútumok csak egy részhalmazát az adatokat.  
* **Összesítési**: az adatok osztása a csoportokba, és a számokat az egyes csoportok tárolni. Például a napi bevétel számokat az elmúlt 20 években éttermi lánc a is összesíti havi bevételeinek, az adatok méretének csökkentése érdekében.  

## <a name="how-to-clean-text-data"></a>Hogyan lehet szöveges adatok letisztázásának?

**Szövegmezők táblázatos adatok** oszlopok igazítási és/vagy a rekord határai befolyásoló karaktereket tartalmazhat. Például a lapok beágyazva egy tabulátorral tagolt fájlhoz OK oszlop átrendeződését, és beágyazott új sor karaktert megszakítja a rekord sorokat. Szöveg írása és olvasása közben a nem megfelelő szöveg kódolási kezelése információk elvesztésének véletlen bemutatása olvasható karakterből áll, például NULL értékek, és előfordulhat, hogy is befolyásolja a szöveg elemzése vezet. Ügyeljen elemzése és szerkesztése érdekében jelenjenek meg a megfelelő igazítási és/vagy a szöveg strukturálatlan vagy félig strukturált adatok kivonat strukturált adatok szövegmezők szükség lehet.

**Adatok feltárása** kínál az korai nézet kialakítása az adatokból. Ez a lépés során adatokkal kapcsolatos problémák számos fedetlen lehet, és megfelelő módszerek alkalmazhatók e kapcsolatos problémák megoldása.  Fontos, hogy mi a hiba forrását, és hogyan a probléma is vezettek például kérdéseket. Ez is segít eldönteni, hogy a megoldásukhoz kell venni az adatfeldolgozás található lépéseket. Milyen típusú egyik szándékozik származtatása az adatok mélyebb is használható fontossági adatfeldolgozás munkamennyiség.

## <a name="references"></a>Hivatkozások

>*Adatok adatbányászati: fogalmak és módszerek*, harmadik kiadás, Péter Kaufmann, Jiawei Han Micheline Kamber és Jian Pei 2011
