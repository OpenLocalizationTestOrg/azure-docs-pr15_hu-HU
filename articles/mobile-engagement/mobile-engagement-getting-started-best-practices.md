<properties 
    pageTitle="Azure mobil tetszés szerint elmélyedhet – első lépések útmutató ajánlott eljárások"
    description="Első lépések útmutató Azure Mobile tetszés szerint elmélyedhet és a gyakorlati tanácsok a bevezetési" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure mobil tetszés szerint elmélyedhet – első lépések az ajánlott eljárások

## <a name="overview"></a>– Áttekintés

**a mobilképernyő túl zsúfoltnak szóköz található:** A 2013-ban tanulmányt található, az átlag mobileszköz 27 telepített alkalmazások volt. Felhasználók általában havonta 30 órát töltött saját alkalmazások. Ez esetben a legtöbb töltött közösségi hálózat és játék (körülbelül 20 óra). 2014-es, amelyet az Android Market webhelyet kellett körülbelül 1,5 millió alkalmazások felhasználók közül választhat. Az Apple store körülbelül 1.2-es millió alkalmazások tartalmazza. Mobilalkalmazás használata továbbra is növekvő, szerint növekvő piaci a fejlesztők versenyt. 

Az átlag mobilfelhasználót telepítése és eltávolítása az alkalmazásokat, attól függően, hogy az érdeklődési megváltoztatása magas gyakorisággal és az alkalmazás találkozik. Annak meghatározásához, az alkalmazás egyik alapvető tudnivalók a hány felhasználóm nem egyszerűen csak az alkalmazás telepítése lesz. Fontos tudnivalók mennyire hasznos az alkalmazást, és ha adott használatát trend változik. A következő kérdéseket válnak fontos:

- Azok a felhasználók a kezdő és keresse meg az alkalmazást, uninteresting vagy a feleslegessé vált? 
- Hány felhasználóm leállt egyáltalán használni? 
- Felfelé vagy lefelé népszerűek az alkalmazás vásárlása?
- Munkafolyamatok elvégzéséhez az alkalmazás vagy kamat hiánya hibák miatt sikertelen felhasználók? 
- Sikerült megőrzi az alkalmazás hasznos és vonatkozó legfrissebb tartalmat szeretne a felhasználó alap közvetítheti? 
- Az összes felhasználó számára ugyanazok a legfrissebb tartalmat szeretne, vagy csak a felhasználói szegmens az alkalmazás a tevékenysége alapján? 
 
Ezek hasonló kérdésekre adott válaszok segítheti a kifejezőképességnek, és a bevételek kiterjesztése az alkalmazás. Is segíthetnek meghatározása és a felhasználó alap megőrzése. 

Médiafájlok kapcsolódó alkalmazások gyakran, hogy az egyes felhasználók között a legmagasabb adatmegőrzési. Ennek több oka, azok is folyamatosan nyújtó legfrissebb tartalmat felhasználók számára. Hasznos leküldéses értesítéseket, ha felhasználói szegmensre irányított korai elfogadása általában alkalmazás adatmegőrzési magas hatással vannak. 

Az Azure Mobile tetszés szerint elmélyedhet program célja kiterjesztése a kifejezőképességnek, és az alkalmazás megőrzése, mert a módszer gyűjtése és elemzése az alkalmazás használatára vonatkozó részletes tájékoztatást nyújt segítséget. Ez segít alap viselkedés megfelelően a felhasználói sorolják be, és hozza létre a fókuszban lévő kampányok azonosított felhasználói vonalszakaszokon végzett módosítások a leküldéses értesítéseket, és az alkalmazás üzenetek kézbesítési. Fő teljesítménymutatók (KPI-k) mérése, az alkalmazás különböző részei hogyan aktív a felhasználók vannak. Azure Mobile tetszés szerint elmélyedhet bemutat a meg kell határoznia a KPI-k. Segítségével növelje a megtérülési ráta megtérülése, mert az infrastruktúra kell fokozhatja a mobilalkalmazással részvételét. 

Hatékony kihasználása Azure Mobile tetszés szerint elmélyedhet, szüksége egy jól megtervezett tetszés szerint elmélyedhet csomaggal indításához. A csomag segítségével azonosítani tudja a részletes adatok engedélyezni szeretné a felhasználó alap szakaszokhoz szüksége lesz. A viselkedés alapulhatnak, és az alkalmazás találkozik. Ahhoz, hogy a csomag sikeres érdemes egyértelműen definiálása, amelyek mérete az alkalmazás céljainak a KPI a legjobb megoldás. Definiált teljesítménymutatók világos, a finom nyomtatott adatgyűjtés használandó elemzése és a KPI-k értékelése az alkalmazását a szükséges logika egyszerűen beágyazhatja. Ez a témakör a gyakorlati útmutató nem a fő teljesítménymutatók a tetszés szerint elmélyedhet csomaggal használni kívánt definiálása. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Lépés: 1: A tipp modell igazítása a KPI-k meghatározása


Helyesen a KPI-k létrehozása, lehet, hogy bonyolult feladat elvégzéséhez. Alkalmazások készült különböző ágazatok van saját jellemzői és céljait. Előfordulhat, hogy ez általában a megközelítés tévessze össze. Ennek elkerülése érdekében célkitűzéseket és fő teljesítménymutatók kell besorolni három fő kategóriába: **üzleti**, a **tevékenységek**és a **technikai**. Ezt nevezzük a **Tipp modell**.

Egy jó terv általában lesz a KPI-k, amely a következő kategóriák tipp modell sokaságbeli mérésére célokat.


#### <a name="business-kpis"></a>Üzleti KPI-k

KPI-k üzleti a legegyszerűbb rész össze kell lennie. Valószínűleg már megadott az egyes űrlap, amikor a mobilalkalmazásban tervezett. A KPI-k mérték bevétel és a BMT általában, alkalmazás súgója. Az alábbiakban néhány példát üzleti KPI-k, melyek segítenek az eredmények mutatói meghatározása közben segíthetnek:

- KPI-k üzleti Media
    - A hirdetések rákattintott száma
    - Felhasználónként felülvizsgálatokra oldal számát
    - Aktuális előfizetések száma
- Játék üzleti KPI-k
    - Az alkalmazás vásárlása száma
    - Átlagos bevétel felhasználónként (ARPU)
    - Minden munkamenetben töltött idő
    - Napok lejátszott és az aktuális játék szint
- E-kereskedelmi üzleti KPI-k
    - Napok alkalmazás használt
    - Átlagos bevétel felhasználónként (ARPU)
    - A kivétel során bevásárlókocsi átlagos összege
    - A legtöbb nézetek és vásárlások termékkategória
- Bank és biztosítási üzleti KPI-k
    - Számlák száma
    - A szolgáltatások aktiválása
    - Meglátogatott ajánlat lapok
    - Való kattintáskor, vagy aktiválva értesítések      



#### <a name="engagement-kpis"></a>Tetszés szerint elmélyedhet KPI-k

Tetszés szerint elmélyedhet KPI-k egy felhasználót a tetszés szerint elmélyedhet mérésére teljesítménymutató. Ezen a területen trendek megállapíthatja, hogy az alkalmazás megőrzésére. Íme néhány példa teljesítménymutatók az ilyen típusú KPI:

- Az utóbbi 7 napból az aktív felhasználók
- Az utóbbi 7 napból inaktív felhasználó száma
- A felhasználók, akik még nem használta az alkalmazás 30 nap száma  

Néhány egyértelmű külső tényezők befolyásolhatják jelölők ezen a területen. Például akkor fontolja meg egy felhasználóval, mindig legyen mobileszközön. Előfordulhat, hogy, vagy a nem lehet igaz. Előfordulhat, hogy általában a egy játék alkalmazást, hogy az ünnepnapok, amikor egy kikét lehet lejátszani iskolai ki- és kikapcsolása munka közben további körül magasabb használatát.   

Jól meghatározott KPI-k ebbe a kategóriába érdemes elolvasnia az alkalmazás és az ügyfelekkel közötti kapcsolatra mérjük.



#### <a name="technical-kpis"></a>Műszaki KPI-k

Ebbe a kategóriába teljesítménymutatók könnyebben eldöntheti, hogy ha az alkalmazás viselkedik megfelelően, függő vagy összeomlik. Hibajelzők is mérje le az alkalmazás állapotának, és megakadályozhatja, hogy a felhasználók a alkalmazással használhatósági problémák határozza meg. Ez a kategória összegyűjtött információkat is tartalmazhat, amely fontos marketing a csoportoknak a teljesítményadatok. Az adatokat is lehet hasznos, ha a hibaelhárítás informatikai és a csoportok nem jelentett hibák azonosításához támogatás. 
 
Íme néhány példa a technikai KPI-k:

- Esetén nem kezelt vagy kezelt kivételinformációk és a szám 
- Az utolsó összeomlik időbélyeg
- Utolsó gombra kattintott, vagy megtekintett utolsó lapja
- Az alkalmazás memóriahasználat
- Alkalmazás keret ráta
- Az alkalmazás futó operációs rendszer verziója
- App verziója

Adja meg ezeket a KPI-k mérték alkalmazás teljesítményének Súgó és a lehetséges hibák pinpoint. A jelölők a segítségére lehetnek egy fix előadása a vevők kell idő csökkentése érdekében. Azok is segítheti, hogy egy felhasználói szegmenst ki egy adott problémák merült fel. A felhasználói szegmens hozhat létre a rendelkezésre álló javításokat és az esetleges promóciók vonatkozó értesítésekkel segíti a felhasználói elégedettséget helyreállítása előadásához kampányok. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook torna 1: A KPI-irányítópult létrehozása

A marketing vonatkozó stratégia megadásakor a KPI-k kell követniük egymást egy nézetet az egyes a fő célja. Pontosan definiált adatpontok, amely lehetővé teszi, hogy figyelheti az alkalmazás és a végfelhasználói viselkedését alapvető információk összegyűjtése kell őket.

KPI-k irányítópult tartalmazó összeállítása a alatti információ

1.  Mik azok a KPI-k alkalmazás?
2.  Mely adatpontok fog minden KPI ábrázolásához használni?
3.  Hol található az alkalmazás (azaz képernyő, beállítások, a rendszer...) a ezeket az adatokat?
4.  Játszhatók le a KPI-Előjegyzéssel sorrendje?

A [Médiafájlok Playbook sablon] is használhatja a **KPI-szerkesztő** munkalap[ Media Playbook link] útmutatást és példákat.



## <a name="step-2-your-engagement-program"></a>Lépés: 2: A tevékenységek Program


Egy nagy mobil tetszés szerint elmélyedhet program figyelembe kell venni az alkalmazás fő része. A teljes mértékben tartalmaznia kell, hogy a felhasználó végrehajtja a formalizálása az első nap során remek üdvözli a program. Ez általában tetszés szerint elmélyedhet és az alkalmazás megőrzése nagyon pozitív hatással lehet. Tanulmányok van jelenik meg, hogy a felhasználók többsége le a telepítés után az első néhány nap alkalmazást használja. Törekedjen felel meg vagy vevői elvárásoknak vezetői kamat, miközben a felhasználó még az alkalmazás összpontosít korai haladja meg a kívánt. Ellenőrizze, hogy a bemutatót tart a kulcs értékét, és az alkalmazás előnyei oda az ügyfelekre. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Leküldéses értesítések a legjobb megközelítés korai Előjegyzések mobileszköz felhasználókkal. Azonban remek ügyelni kell, amikor a felhasználók a leküldéses értesítések példájaként. Mivel a kap, amikor a felhasználó érzi, például a levélszemét és az uninteresting értesítéseket, komoly hatással lehetnek. Néhány kattintással a felhasználó törlése Előfordulhat, hogy az alkalmazás nem való visszatéréshez. A felhasználó erősen személyre szabott az alkalmazás érték helyett általános levélszemét még kap.

Miután felhasználók aktívan folytat, majd a tetszés szerint elmélyedhet program segíthetnek meghajtó egyéb szempontok az alkalmazás.

Például a egy marketingkampány értékeljék az alkalmazás az aktív felhasználók kérő sikerült telepítő. Mivel a felhasználói szegmens a legaktívabb és van Önnel a legtöbb élmény alkalmazást, volna várt a legpontosabb minősítése. Ha befejezte a magas alkalmazás minősítés, segítheti a meghajtó fel az alkalmazást az új ügyfél WIA költségeket, valamint csökkentése szerves letöltését.



#### <a name="engagement-sequence"></a>Tetszés szerint elmélyedhet sorrend


Globális tetszés szerint elmélyedhet Program különböző tetszés szerint elmélyedhet sorozatok tartalmazza. Az egyes célja több célok eléréséhez.


###### <a name="life-push-sequence"></a>Élettartam leküldéses sorrend


Az egy élet leküldéses sorozathoz célkitűzései eltérő attól függően, hogy a felhasználó tetszés szerint elmélyedhet a alkalmazással életciklusának. Lehet, hogy egy felhasználó új, inaktív vagy nagyon aktív. Egy tetszés szerint elmélyedhet életciklus különböző szakaszaiban felhasználók is élvezhetik a legfrissebb tartalmat tippek vagy hivatkozások formájában dokumentációt. 

Például új felhasználó lehet szüksége segítséget az első lépések az alkalmazás, vagy egy új felhasználót arra ösztönzi az alábbihoz hasonló élvezhetik az első alkalommal azok indítsa el az alkalmazást...

*"Neked, hogy fedélzeti! Ne feledje, hogy a 1st hónap ingyenes megszerezni bejelentkezési!"*


###### <a name="behavioral-push-sequence"></a>Viselkedési leküldéses sorrend

A viselkedési leküldéses sorrendjének célja, hogy az alkalmazás gyűjtött felhasználói tevékenysége alapján használatát növelése.  

Egy fantasy labdarúgó alkalmazás nagyon aktív felhasználó például összekapcsolhatók előfordulhat, hogy a leküldéses értesítést követő részt...

*"János Ön egy komoly labdarúgó ventilátorral! Jelentkezzen be a NFL szakaszt, és ingyenes hozzáférést a SuperBowl win!"*


###### <a name="alerting-push-sequence"></a>Leküldéses szekvencia riasztási

Felhasználók csak a saját érdeklődési vonatkozó híreket fogják értékelni. Egy figyelmeztető leküldéses sorozat tetszés szerint elmélyedhet hatékonyabbá tehető az értesítések alapján érdekében a felhasználó rendelkezik jól láthatóan küldésével. Ennek oka lehet explicit, amikor egy felhasználó kijelöli a saját érdeklődési a alkalmazásban. Azt is is meghatározni implicit módon – az alkalmazás felhasználói beavatkozás során gyűjtött adatok alapján.

A felhasználót az E-kereskedelmi alkalmazás például rendszeresen előfordulhat, hogy vásároljon egy adott márka, kávézóból, amely a KPI-k üzleti rögzített. Az alábbi értesítés bővítheti a felhasználó tetszés szerint elmélyedhet a alkalmazással.
 
*"Hi: Wes, kávézóból, a kedvenc márka közül a is eladási ki az első hete 2015 szeptember 25 %-át. Nagyra értékeljük, mint egy ügyfél, és győződjön meg arról, hogy megy végbe, tudomása volt."*

###### <a name="rentention-push-sequence"></a>Rentention leküldéses sorrend

A sorrend az a célja, hogy a felhasználók a egy ismétlődő leküldéses értesítést kampányok segíti meghajtó egy normál hogy érdekes – az alkalmazás megőrzi. Ez segít növelése alkalmazás adatmegőrzési, ha a felhasználó rendelkezik a kapcsolati. 

Ha például a sports kapcsolódó alkalmazás a felhasználó előfordulhat, hogy az alábbihoz hasonló leküldéses értesítés heti alapján a felhasználó a kedvenc csoportok:

*"Az win 200 pontot a névjegyadatok, kattintson a szavazás, hogy a New York Yankees lesz az e héten Toronto kék Jays szemben a mérkőzés szavakat win!"*


#### <a name="the-3w-approach"></a>A 3W megközelítés

A különböző leküldéses sorozatok Mastering segítséget nyújt a végfelhasználók vesznek. Azonban van szüksége az értesítések testreszabása a 3W megközelítés használható. A 3W megközelítés kell foglalkoznia, ki mit és mikor minden értesítést. Ha pontosan felel meg ezeket a kérdéseket, értesítések kell megfelelően fordítani tetszés szerint elmélyedhet a.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Ki: A felhasználói szegmens, üzenetek

Értesítések terjesztése a felhasználók figyelembe kell venni nagyon érzékeny kommunikációs csatornát. Győződjön meg arról, hogy az adott felhasználói szegmens érdekében jól korlátozódik célja a felhasználói szegmensre küldendő értesítéseket. Helytelenül útválasztásos értesítjük valószínűleg a nagyon egy negatív hatással van a felhasználó. Azok tekinthetik, az alkalmazás eltávolított vezető levélszemét. 

Meghatározott technikai és viselkedési feltételek kombinációi, amelyek értesítést kapnak felhasználói szegmens megadásakor használata Egy egyszerű példa egy felhasználói szegmenst definiálása az alábbi utasítás hasonló lehet:

"A felhasználók, akik indított a az első egy mobilalkalmazás 3 nappal korábban idő és a bejelentkezési lapja van megtekintett kétszer nélkül bejelentkezési ténylegesen befejezése".
 
A kimutatás segít a kimutatásadatokat egy adott forgatókönyvet támogatja a gyűjtendő adatok azonosításához.


###### <a name="what-the-message-that-you-will-send"></a>Mi: Az üzenetet, amelyet elküldjük

**Hang**

Egy megfelelő hangjelzést használja az előjegyzések a szegmentált felhasználóknak. Az növekedni biztosan kapcsolatba lépni a végfelhasználók számára, és az alkalmazás a felhasználó érdekes előléptetése jó módszer. 

**Átirányítása**

Leküldéses értesítést használható az alkalmazás több, mint megnyitása. Ha az értesítő üzenet biztosítja a hátteret, például a közvetített hírek vagy egy termék akció, ez az értesítés előfordulhat, hogy közvetlenül a megfelelő tartalmat az alkalmazáson belül mély hivatkozást. Támogatja ezt, létre kell hoznia egy URL-cím színsémát, hogy az alkalmazás kezelése az átirányítást. Munka a tevékenységek sorrendek esetén az, hogy nem kell elfelejtett fontos lépést.

Átirányítás a más rendszerekben is kezelhetők. Ha például művelet URL-címének lehetősége végfelhasználók átirányítása számos más rendszerek, többek között az alábbi:

- A webhely
- A postaláda az e-mailek már be van állítva
- Az SMS-mezőben
- A telefonos szolgáltatás
- Közvetlenül az alkalmazás tárolása értékelése az alkalmazást. 

Ezzel a megoldással sok lehetőségeket folytasson végfelhasználók és a teljesítmény javítása érdekében automatikus szabályok létrehozása.


**Formátum és tartalom**

Különböző típusú és a leküldéses értesítést formátumok:

1. **Hirdetmények** : lehetővé teszi, hogy hirdetési üzenetek küldése a felhasználóknak a különböző percet (ki letölthető alkalmazásban vagy bármikor).
2. **Lekérdezések** : engedélyezve, hogy a végfelhasználó által kérdések felvetése információk összegyűjtése. Adott válaszok majd érhetők el, a cél végfelhasználóknak feltétel létrehozásakor.
3. **Adatok verembe küldi** : lehetővé teszi, hogy az alkalmazás frissítéséhez bináris vagy base64 adatok fájl küldése. Adatok leküldéses lévő adatokat küldi el az alkalmazást, hogy a felhasználói környezetének személyreszabása az alkalmazásban. Az alkalmazás kell az adatokat támogatja az adatok leküldéses kell megtervezni.
4. **Csempék (csak a Windows Phone)** : engedélyezve van a Microsoft leküldéses értesítést szolgáltatás (MPNS) segítségével küldje el a natív leküldéses értesítést tartalmazó XML-adatok (óta SDK 0.9.0 verziója támogatott. A végleges tartalom mozaikokhoz nem haladhatja meg a 32 kilobájt.). Az üzenet közvetlenül a fal csempe megjelenik.
5. **Webnézet** : webes nézet egy előugró tartalmazó webes tartalmak. Ez az előugró jelenik meg, ha a végfelhasználói rákattint a leküldéses értesítést. Webes nézet teszi lehetővé, hogy a végfelhasználói további interakció.
 
>[AZURE.NOTE] Győződjön meg arról, hogy a leküldéses értesítések küldi tartalom megfelel-e a megfelelő platform (iOS, Android, a Windows) szabályai alkalmazások fejlesztéséhez, és a leküldéses értesítések küldése.

 


###### <a name="when-the-timing-of-your-campaign"></a>Mikor: A marketingkampány időzítésének

Amikor indítja el a legjobb időpontot tudja aktiválni a marketingkampány leküldéses értesítéseket? Meg kell a kézi és automatikus? Érdemes azt ismétlődő? Annak megállapítása, a jobb oldali idő vagy a gyakoriság elengedhetetlen a legjobb eredményt rendelkező felhasználók végzését. Az egyes tetszés szerint elmélyedhet sorrend és forgatókönyv, meg kell adnia mikor lesz a legjobb időpontot tudja küldeni a leküldéses értesítéseket. Néhány példával is lehetséges:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Ha szeretné elküldeni a sok értesítések naponta, végre kell hajtania komoly figyelembe, hogy a a felhasználók lehet-e lássák a levélszemétként kommunikáció. 

Azure Mobile tetszés szerint elmélyedhet itt kétféleképpen esik, hogy észlelt kommunikáció elkerülése érdekében. Első lépésként finom szemek szegmens segítségével győződjön meg róla, akkor nem célként ugyanazokat a felhasználókat. Azure Mobile tetszés szerint elmélyedhet emellett egy "kvóta" funkció. Ez a funkció korlátozhatja a marketingkampány küldött értesítések. Például egy alapértelmezett kvóta beállítása a heti 5 biztosítja, hogy egy felhasználó, valamint a marketingkampány felhasználói szegmens részét legfeljebb 5 értesítés érkezik, a hét.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook torna 2: Adott tetszés szerint elmélyedhet-alkalmazás létrehozása

Töltsön egy kis időt, a célok összesítését, és a várt használata bizonyos szekvencia lejátszásához kampányok definiálása. Ellenőrizze, hogy alkalmaz a 3W megközelítés az értesítéseket a kampányok. 

A **Program tetszés szerint elmélyedhet** munkalap használja a [Médiafájlok Playbook sablon] [ Media Playbook link] útmutatást és példákat.


## <a name="step-3-app-integration"></a>Lépés 3: App integrációja


#### <a name="create-a-tag-plan"></a>Címke terv létrehozása

Azure Mobile tetszés szerint elmélyedhet integrálása az alkalmazásban, meg kell címke terv létrehozása. A címke csomagja a kiindulópont a projekt. Azt határozza meg a marketingtevékenység jellemzői, a munkafolyamat az alkalmazás és a KPI-k mérésére alkalmazásban valós címke adatgyűjtési közötti kapcsolatra. Azt jelzi, hogy milyen analytics lesz láthatja a portálon. Azt is segítségével határozza meg a felhasználói szegmens, majd küldje el a fókuszban lévő leküldéses értesítéseket folytatni a végfelhasználók számára. Egyszer a címke csomaggal rendelkezik definiálva, hozzáadása, a kód integrálnia kell az alkalmazás az egyszerű az Azure Mobile tetszés szerint elmélyedhet SDK használatával.

A címke-csomagot kell nem címke minden az alkalmazásokban. Címke adatainak a mobil tetszés szerint elmélyedhet vonatkozó stratégia részét képező csak tartalmaznia kell. Ez valószínűleg különböző alkalmazás között. A [Médiafájlok Playbook sablon] [ Media Playbook link] feltéve által Azure Mobile tetszés szerint elmélyedhet segít generál címke terv egy adott metódussal. A **Címke terv** munkalap egy segédvonalat a címke Épületterv használja.

A munkalap címke szakasz megadásakor kell nagyon konkrét. Ez nagyon fontos fejetlenséget elkerülése érdekében. Minden részlet, amelyben minden egyes címke küld forgatókönyv várható. A tevékenység, ahol minden címke beágyazott nevét tartalmazza. Ez az összes szerepelnie kell a munkalap **Informative** részét. A címke terv munkalap próba-ellenőrzési fő ismertetője kell lennie. 

**A gyűjtendő adatok** csoportban a fejlesztőcsapatához keresse a típusok, nevek, értékek és extra-információ kulcs/érték párokká, az egyes fog ágyazhatók be az alkalmazás címkét szükséges.

Azt javasoljuk, hogy a címke-csomagot megtekintésével és a projekttel kapcsolatos összes csoport. Szükséges javítások elvégzése, és ellenőrizze, hogy minden adat törlése a marketing- és a csoportoknak.

A **munka kimutatás** munkalap használható, hogy segítségével iránymutatást minden résztvevője a projektben.


#### <a name="data-types"></a>Adattípusok

Ezek az adatok támogatásával Azure Mobile tetszés szerint elmélyedhet elterjedt típusú.

###### <a name="devices-and-users"></a>Eszközök és a felhasználók

Azure Mobile tetszés szerint elmélyedhet létrehozása egy egyedi azonosítót az egyes felhasználók azonosítja. Az azonosító neve eszközazonosítót (vagy deviceid). Oly módon, hogy ugyanazon az eszközön futó összes alkalmazások megosztani az azonos eszközazonosítót jön létre.

###### <a name="sessions-and-activities"></a>Munkamenetek és a tevékenységek

A munkamenet az alkalmazás a felhasználó által futtatott egy példánya. A munkamenet átnyúlik a időpontot, a felhasználó elindítja az alkalmazást, amíg az megáll a.

Egy tevékenység egy sor olyan lehetőségek az alkalmazás előfordulhat, hogy a munkamenet közben logikai csoportja. Általában egy adott képernyő az alkalmazásban, de semmi határozza meg az alkalmazás a logika lehet. Legalább meg kell nyomon követése minden képernyő vagy a tevékenység az alkalmazás. Ez lehetővé teszi, a felhasználói elérési megértéséhez.


###### <a name="events"></a>Események

Jelentés felhasználói beavatkozás – az alkalmazás események használhatók. Ez lehet azonnali műveletek, például a tartalom megosztása az vagy videoklip indítását. Események címkézés szolgáltatja adatok megjelenítése, hogy a felhasználók hogyan kezeljék az alkalmazás gyűjteményei. 

###### <a name="jobs"></a>Feladatok

Időtartammal rendelkező műveletek jelentés feladatok használhatók. Néhány példa tartalmazza:

- Az API-hívások végrehajtása
- Hirdetések megjelenítési ideje
- Háttér tevékenységek időtartamának 
- Vásárlás folyamat időtartam
- Videó megtekintése


###### <a name="errors"></a>Hibák

Hibák hibát észlelt a által használhatók. Ha például nem a megfelelő felhasználói műveletek, és API hívás sikertelen.

###### <a name="application-information"></a>Alkalmazás adatai

Alkalmazás (App – útmutatók) adatok a felhasználói élmény alkalmazással kapcsolatos adatok címkézéséhez. Az alkalmazás a felhasználó interakció hozza létre. 

Egy adott app – útmutatók billentyű Azure Mobile tetszés szerint elmélyedhet csak nyomon követi az a legkésőbbi érték (nincs Előzmények). App – útmutatók megjelennek az alkalmazás és a végfelhasználók állapotát. Példa a bejelentkezési állapot, vagy a felhasználó kedvenc termékcsoport.

###### <a name="crash-data"></a>Adatok összeomlik

Az alkalmazás nem kezeli Mobile tetszés szerint elmélyedhet SDK jelentések alkalmazáshibák által automatikusan gyűjtött adatok összeomlik. Ha például egy esetén nem kezelt kivétel, amely akkor következik be.


###### <a name="extra-data"></a>További adatok

Paraméterek eseményeket, a hibák, a tevékenységek és a feladatok bővíthetők. Ez a extra adatokat egy fejlesztő biztosíthatják meghatározott adatokat az alkalmazásból. Az egyedi szegmens definiálása fontos. 

Például az érték az "cikk" címke lehetővé teszi, ki az adott cikkben megtekintett alapján szakasz végfelhasználók számára. Jó helyen jár, hogy nem lehet elég. Hatékonyabb lehet, ha ugyanazt a "cikk" címkét megtalálható extra-információ, például "news_category" egy tevékenységen belül. Ez a felhasználó a kedvenc kategóriák dinamikusan megállapításához segítségére lehetnek. 

Extra-információ kulcs/érték párban kell jelenteni. A példában az media alkalmazáshoz a "news_category" extra-információkkal akkor is, hogy a kategória értéke. Például: "a sports", "fogyasztási" vagy "politikában".





#### <a name="tag-and-sdk-integration"></a>Címke és SDK integrációja 

Lépésenkénti utasításokat az Azure Mobile tetszés szerint elmélyedhet SDK integrálása az alkalmazást kövesse a [Tetszés szerint elmélyedhet SDK integrációs](mobile-engagement-windows-store-integrate-engagement.md) dokumentációt Azure webhelyen. Válassza ki a cél platform a hivatkozásokat, hogy a lap tetején.

Azt javasoljuk, hogy a két alkalmazás Azure Mobile tetszés szerint elmélyedhet épülő projektek létrehozása. Egy fejlesztés és próba előkészítése és a másik gyártási átmeneti. Informatikai csapata is majd előléptetése a próba átmeneti termelési amikor a felhasználó elfogadó tesztelése sikerrel jár.



#### <a name="user-acceptance-testing-uat"></a>Felhasználói elfogadási (UAT) tesztelése

Gondoskodhat arról, hogy, hogy megfelelően működik-e minden felhasználó elfogadó tesztelése (UAT) jár. Munkafolyamatok el tud végezni, és a címke csomagja az összes szükséges adatok összegyűjtése:
 
- Információk címkézés kell megfelelően dokumentált AZME fogalmak helyen
- Szükséges minden információt összegyűjtött (További információ érték, alkalmazás információ érték is beleértve)
- Nómenklatúra egyezés szerint elrendezés címke megtervezése
- Nem ismétlődő címkéket küldött nem

Az alkalmazás a beágyazott viselkedését típusú alapos tesztelése

- Hirdetmények, felmérésekre, adatok verembe küldi ki és a alkalmazást
- Szöveg és Web-nézetek
- Jelvény frissítése, a kategóriák



#### <a name="setup"></a>A telepítő

Azure Mobile tetszés szerint elmélyedhet beállítása rendkívül egyszerű. A felhasználói felület kapcsolódó dokumentumokat eljuttatja a Azure Mobile tetszés szerint elmélyedhet webhelyén, [hogy miként navigálhat a felhasználói felület](mobile-engagement-user-interface-home.md)érhető el.

Indítsa el a megfelelő szerepkörök és a felhasználók a projekt szerepköreit beállításával ajánlott. Ezzel az elemzéssel kezelheti a megfelelő hozzáféréssel a platform minden felhasználó számára. A szerepkör a következők lehetnek:

- A rendszergazdák
- A fejlesztők
- A nézők 

Utána:
- Regisztráljon az deviceID tesztelje a saját eszközön.
- Nyissa meg a fiók beállításait, és az időzóna beállítása, hogy a diagramok és az időzóna beállítása értesítés marad.
- Nyissa meg a beállításokat, az alkalmazás, és a "App – útmutatók" cél végfelhasználói elérhető kell regisztrálni.

Az első leküldéses értesítést marketingkampány futtatásával kapcsolatos további tudnivalókért tekintse át a [kezdőlépéseket kell megtennie használata és kezelése veremmutatót kattintva keresse meg a felhasználóknak](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Elfogadásáról


Tetszés szerint elmélyedhet programok közelítéses, és el kell folyamatosan javulást Öné, mi a legjobban az alkalmazás kísérletezhet. 

Első lépésként közben a yammert az tetszés szerint elmélyedhet stratégiák nem generál egy teljes globális tetszés szerint elmélyedhet stratégia próbál elkészítésének. A lépésenkénti megközelítés a KPI-k és hogyan használhatja őket, hogy. Tetszés szerint elmélyedhet stratégia egyedi minden alkalmazás lesz.

Miután néhány folyamatok kialakítása akkor is érdemes megfontolni a következő hozzáadása a tevékenységek programok:

- Nyomon követés: Felhasználók finomítása, és valószínűleg definiálása adatgyűjtés adatforrásból. Azure mobil tetszés szerint elmélyedhet adatgyűjtés adatforrások csatolható. Lehetővé teszi a Lync-minden forrás előadásokhoz. Az információk segítségével a WIA befektetés maximalizálása lesz. 

- A és B tesztelése: Ez része-alapvető tevékenységek program. Mindegyik alkalmazásra van a saját adatai. A tesztelés B, a tetszés szerint elmélyedhet program javíthatja /.

- GEO-hely: Ez a márka egy nagy lehetőséget. Ez a funkció több érheti el a megfelelő helyre és időpontban. Azt javasoljuk, hogy elég végfelhasználói viselkedés adatainak összegyűjtötte geo helyfüggő használatának megkezdése előtt ellenőrzéséhez.

- Leküldéses adatok: adatok leküldéses egy láthatatlan leküldéses. Adatok leküldéses lehetővé teszi, hogy a végfelhasználói tevékenysége alapján alkalmazás testreszabása. Például ha felhasználói szegmensre gyakran olvas magas technológiájú termékek, az alkalmazás tulajdonosa fog személyre szabhatja a saját kezdőlapja kiváló technológiájú tartalom, amelyben adatok leküldéses küldhet.



## <a name="next-steps"></a>Következő lépések

- [Azure Mobile tetszés szerint elmélyedhet fiók létrehozása](mobile-engagement-create.md).
- Látogasson el a [meghatározása a Mobile tetszés szerint elmélyedhet vonatkozó stratégia](mobile-engagement-define-your-mobile-engagement-strategy.md) , ha többet szeretne tudni a Mobile tetszés szerint elmélyedhet vonatkozó stratégia definiálásáról.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
