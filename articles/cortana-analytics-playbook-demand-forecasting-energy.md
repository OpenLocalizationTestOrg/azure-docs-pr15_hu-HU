<properties
    pageTitle="Cortana intelligencia megoldás sablon Playbook energy igény szerinti előrejelzéshez |} Microsoft Azure"
    description="Megoldás sablon intelligenciával Microsoft Cortana, amely segít a előrejelzés egy energy segédprogram céget igény szerint."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana intelligencia megoldás sablon Playbook Energy igény szerinti előrejelzéshez  

## <a name="executive-summary"></a>Összefoglalás  

Az elmúlt néhány év dolog Internet (IoT), alternatív energiaellátó források és nagy adatok döntő lehetőségek létrehozása a segédprogram és energiaellátó tartományban van egyesíti. Egy időben az eszköz és a teljes energy szektor van látható fogyasztói igénylő hatékonyabb energiát is használatuk szabályozhatja a simítási felhasználás. Emiatt a segédprogram és intelligens rács cégek van nagyszerű kell feltárásáról és megújítása magukat. Ezenkívül sok hatvány és segédprogram rácsok egyre elavult és nagyon költséges karbantartása, illetve kezelhet. Az utolsó évben a csapat a energy tartományon belül Előjegyzések számú dolgozik. Ezek az előjegyzések során azt, amelyben a segédeszközök vagy ISV (független szoftver szállítók) van már megjeleníti az előrejelzés a jövőbeli energy igény szerinti sok esetben merült fel. Ezek az előrejelzések fontos szerepet az aktuális és a későbbi üzleti, és az egyes használati eset alapja váltak. Ezek közé tartozik, rövid és hosszú távú power betöltés előrejelzés, kereskedési, terheléselosztás, a rács optimalizálási stb. Nagy adatok és a speciális Analytics (ε) módszerek például gépi tanulási (Machine Learning) a fő enablers pontos és megbízható előrejelzések készítésére.  

Ez a playbook azt összeállított az üzleti és az analitikai irányelvek sikeres fejlesztési szükséges, és energy igény szerint a telepítésének előrejelzési megoldás. Segédprogramok, adatok tudósok és adatok mérnökök teljesen operationalized, felhőalapú, igény szerint előrejelzéshez-megoldásokkal létrehozásáról az alábbi javasolt irányelveket segítséget. Azok a vállalatok, akik csak most kezdi a nagy adatok és a fejlett analitikai utazás ilyen megoldás a kezdeti rendező a hosszú távú intelligens rács stratégia is képviseli.

>[AZURE.TIP] Diagram, amely építészet áttekintést nyújt az ezzel a sablonnal letöltéséhez látogasson el [az igény szerinti előrejelzéshez energiát Cortana üzletiintelligencia-megoldás sablon architektúra](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>– Áttekintés  

A dokumentum bemutatja az üzleti adatok és technikai szempontokat Cortana intelligencia használata és az adott Azure gépi tanulási (AML) végrehajtására és energiaellátó előrejelzés megoldást üzembe. A dokumentum három fő részből áll:  

1. Üzleti ismertetése  
2. Adatok ismertetése  
3. Technikai végrehajtása

A **Vállalati ismertetése** rész az egyik dátumtáblázatok ismertetése és előtt, hogy egy befektetés döntés figyelembe kell üzleti méretarány ismertet. Ismerteti ahhoz, hogy kéznél annak érdekében, hogy prediktív analitikai és gépi tanulási valóban hatékony és vonatkozó üzleti probléma. A dokumentum további gépi tanulási, és hogyan használják energy előrejelzés problémák megoldására alapjait ismerteti. Ismertet a használati eset minősítési kritériumok és a vonatkozó követelmények. Néhány példa használata esetek és az üzleti terv esetek is rendelkezésre állnak.

Adatai a fő összetevő-gépi tanulási megoldás. A jelen dokumentum **Adatok ismertetése** részét néhány fontos szempont az adatok foglalkozik. Ismertet a típusú adat energy előrejelzés, adatok minőségi követelmények, és milyen adatforrások általában léteznek van szükség. Is bemutatják, hogy valójában meghajtó modellezési rész adatfunkciók előkészítése az nyers adatokból használatának.

A dokumentumot a harmadik rész ismerteti azokat a megoldást az a **Technikai végrehajtása** funkcióival. Szolgáltatás mérnöki és modellezése a adatok tudományos folyamat alapvető és néhány részletesen ezért tárgyalt. Webszolgáltatások, amelyek egy fontos gépjármű prediktív analytics-megoldások a felhőben telepítéshez fogalmának lefedi. Azt is szerkezeti egy tipikus architektúráját egy, a végpontok közötti operationalized megoldást.

Ezenkívül a dokumentum tartalmaz-anyagminta további tudnivalók a tartomány és technológia eléréséhez használható.

Fontos tudni, hogy azt nem kíván terjed ki ezt a dokumentumot az adatok mélyebb tudományos folyamat a matematikai és technikai szempontjait. Ezeket az adatokat az [Azure Machine Learning dokumentáció](http://azure.microsoft.com/services/machine-learning/) és [blogok](http://blogs.microsoft.com/blog/tag/azure-machine-learning/)találhatók.

### <a name="target-audience"></a>Célközönség   
Ehhez a dokumentumhoz a célközönség üzleti és a műszaki személyzet ki szeretné megpróbálhatnak és gépi tanulási megértése alapuló megoldásokat, és hogyan ezeket használják konkrétan a energy előrejelzés tartományon belül.

Adatok tudósok a magas szintű folyamat, amely egy energy előrejelzés megoldást üzembe helyezésével meghajtók megértéséhez szükséges a dokumentumot olvasó is előnyeivel. Ebben a környezetben is használhatná jó alapterv létrehozására és kiindulási pont további részletes és speciális anyag-e.

### <a name="industry-trends"></a>Trendek  
Az elmúlt néhány év IoT, alternatív energiaellátó források és nagy adatok létrehozása nagy lehetőségek segédprogram és energiaellátó térköz van egyesíti. Egy időben az eszköz és a teljes energy szektorok van látható fogyasztói igénylő hatékonyabb energiát is használatuk szabályozhatja a simítási felhasználás.

Számos segédprogram és intelligens energy cégek van már pioneering a [Rács intelligens](https://en.wikipedia.org/wiki/Smart_grid) egy számot a rács által generált adatok felhasználása esetek, amelyek a használati telepítésével. Elektromos gyártási járó jellemzőinek köré szerveződik sok használati eset: nem lehet halmozott sem tárolt különítsen készletértékét. Igen mi elő kell használni. Hatékonyabban kívánt segédprogramok kell energiafogyasztást egyszerűen előrejelzési mivel, amely fogja őket nagyobb azt jelenti, hogy **Egyenleg készletek és igények**, megelőzve energy veszteség **csökkentése üvegházhatású gázok kibocsátási**és vezérlő költség.

Beszélgetés költségek, ha van egy másik fontos eleme, amely ár. Új tulajdonságainak kereskedelmi power segédprogramok között van nagy szükség van **az előrejelzési jövőbeli igény szerinti és a későbbi ingatlanok árának az átlaga elektromos**vett fel. Ez segít a vállalatok, akik a termelési kötet határozza meg.

Az "intelligens" szó használatakor ténylegesen hivatkozunk, amelyek képesek ismerje meg, és végezze el az előrejelzések a rács. Azt cikket is annak felméréséhez szezonális változásai felhasználási, valamint **előre ideiglenes túlterhelés azokról a helyzetekről, és azt automatikusan beállíthatja**. Által távolról szabályozó felhasználás (ezek intelligens méter segítségével), honosított túlterhelés helyzetekben kell kezelni. **Először előrejelzésére, és ezután eljáró**, a rács teszi magát végezhesse munkáját idővel.

A dokumentum azt, hogy pontosan illeszkedjen a jövőben az előrejelzés használati eset adott családját összpontosít hátralevő rövid kifejezés és a hosszú távú energy igény szerint. Azt néhány hónapig az ezekre a területekre dolgozik, és néhány ismeretek és a szakértelem, amelyek lehetővé teszik az üzleti minőségű eredményt adja, hogy mi szerzett. Más használati eset vonatkozik, valamint a dokumentumban a közeljövőben.

## <a name="business-understanding"></a>Üzleti ismertetése

### <a name="business-goals"></a>Üzleti célok
A **Energy bemutató** célja, hogy a bemutatják egy tipikus prediktív analytics és gépi tanulási nagyon rövid idő keret telepített megoldás. Az aktuális fókusz többek között a munkafüzetek engedélyezéséről energy igény szerinti előrejelzési megoldások, hogy az üzleti értékére is gyorsan történjen realizálása, és kapcsolatos károkozásra alapján. A playbook az adatokat az ügyfél, a következő célok végrehajtásának segíthetnek:
-   Rövid idő gépi tanulási értékének alapú megoldás
-   Azt jelenti, hogy bontsa ki a próbaüzem használatieset üzleti van szükségük alapján szélesebb hatókör vagy más használati eset
-   Gyorsan megpróbálhatnak Cortana üzletiintelligencia-csomagja termék

Az alábbi kitűzött célok e playbook célja, hogy az üzleti és műszaki ismeretek, amely segítséget nyújt az alábbi célok eléréséhez.

### <a name="power-load-and-demand-forecasting"></a>Power betöltése és igény szerinti előrejelzéshez
A energy szektor belül is okozhatja számos módon, mely igény szerint az előrejelzés segítséget kritikus üzleti problémák megoldásában. Valójában igény szerint az előrejelzés lehet tekinteni az alapvető használatára sokszor a iparágban alapja. Az általános kétféle energy igény szerinti előrejelzések javasolt: rövid és hosszú távú. Előfordulhat, hogy mindegyik más célt szolgálnak, és egy másik megközelítés csatlakozást. A fő a kettő közötti különbség az előrejelzési horizont, ami azt jelenti az időtartományt, a jövőbe, amelynek azt volna előre jelzett.

#### <a name="short-term-load-forecasting"></a>Rövid kifejezés betöltés előrejelzéshez
Rövid kifejezés betöltése előrejelzés (STLF) energy igény szerinti keretén belül definíciója összesített be, hogy a közeljövőben, kattintson a rács (vagy egészének a rács) különböző részeihez előrejelzett van. Ez a helyi rövid idő horizont és 24 óra 1 óra értéket tartományát belül kell van megadva. Egyes esetekben egy horizont 48 óra is lehetőség. STLF ezért egy műveleti használatieset a rács a gyakori. Íme néhány példa a STLF alapú használati eset:
-   Forrás-és igény szerinti terheléselosztás
-   A Power kereskedelmi támogatás
-   Piaci tétele (beállítás power ár)
-   Rács műveleti optimalizálása
-   [Igény szerint válasz](https://en.wikipedia.org/wiki/Demand_response)
-   Maximális igény szerinti előrejelzéshez
-   Oldalsó igénykezelés
-   Terheléselosztás és Túlterhelés megakadályozása
-   Hosszú távon a betöltés előrejelzéshez
-   Hibafa és rendellenességet észlelési
-   Csúcs megszorítás/simítás 

STLF modell főleg alapuló közeli múltbeli (utolsó napi vagy heti) felhasználási adatok és használatának előrejelzett hőmérsékleti egy fontos előrejelző szerint. A következő óra előrejelzés pontos hőmérsékleti beszerzése és be és 24 óra egyre kisebb bonyolulttá most nap. Ezek a modellek kisebb érzékenyek szezonális mintázatok vagy hosszú távú felhasználás trendek.

SLTF megoldások is valószínűleg készítése a nagy mennyiségű előrejelzési hívások (a szolgáltatási kérelmek), mivel ezek vannak meghívott óránkénti kombinálásával és bizonyos esetekben még nagyobb gyakorisággal. Érdemes emellett gyakori beültetésre, ahol minden egyes állomás vagy átalakító megfelelője önálló modell és így a szövegbevitel kérések mennyiségű még nagyobb megjelenítéséhez.

#### <a name="long-term-load-forecasting"></a>Hosszú távon a betöltés előrejelzéshez
A cél, a hosszú távú betöltés előrejelzés (LTLF), az előrejelzések power igény szerint egy idő horizont 1 hét kezdve, több hónapra (vagy egyes esetekben az évek számát). A horizont tartománya főleg vonatkozó tervezési és befektetés használjon esetekben.

Ha a hosszú távú esetben fontos kiváló minőségű adatot, amely magában foglalja a módosítva több éves (legalább 3 év). Ezek a modellek fog általában szezonalitás mintázatok kinyerése a korábbi adatok, és használja az külső predicators ilyen, időjárás, valamint a környezet mintázatok.

Fontos, hogy a hosszabb van az előrejelzési horizont minél kevésbé pontos az előrejelzés, előfordulhat, hogy ezek sok. Ezért fontos, hogy néhány konfidencia együtt a tényleges előrejelzés, amelyek lehetővé teszik, hogy az emberek kiszámíthatja a lehetséges változat a tervezési folyamat kiszámítására.

Mivel a felhasználás forgatókönyv LTLF főleg tervezi, megakadályozási is sok alsó előrejelzési kötet (mint STLF). Azt volna általában lásd: ezek az előrejelzések képi megjelenítés eszközöket, például az Excel vagy a PowerBI beágyazására, és a felhasználó által manuálisan indított.

### <a name="short-term-vs-long-term-prediction"></a>Rövid kifejezés összehasonlítása hosszú távú előrejelzési
Az alábbi táblázat a legfontosabb attribútumok szempontból STLF és LTLF hasonlítja össze:

|Attribútum|Rövid betöltés előrejelzése|Hosszú távon az előrejelzés betöltése|
|---|---|---|
|Előrejelzés horizont|Az 1 óra, 48 óra|6 hónap vagy több 1|
|Adatok Granularitás|Óránkénti|Óránkénti vagy a napi|
|Tipikus használati eset|<ul><li>Igény szerint/ellátási terheléselosztás</li><li>Válassza az óra előrejelzéshez</li><li>Igény szerint válasz</li></ul>|<ul><li>Hosszú távon tervezési</li><li>Rács eszközök tervezése</li><li>Erőforrás-tervezés</li></ul>|
|Tipikus kiszámítására|<ul><li>Napi vagy heti</li><li>A nap óra</li><li>Óránkénti hőmérsékleti</li></ul>|<ul><li>Év hónapja</li><li>Hónap napja</li><li>Hosszú távú hőmérsékleti és környezet</li></ul>|
|Korábbi adattartomány|Két vagy három év értékű adatok|5 – 10 év értékű adatok|
|Tipikus pontosság|MAPE * 5 %-os vagy az alsó|MAPE * 25 %-os vagy alsó|
|Előrejelzési gyakorisága|Óránként vagy 24 óránként előállítása|Miután havi, negyedéves és éves előállítása|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – középérték átlagos százalékos hiba

Ebből a táblából is láthatja, ahogy fontos igazán megkülönböztetni rövid és a hosszú távú esetek előrejelzés, ezek jelenítik meg különböző az üzleti igények és különböző telepítési és fogyasztási lehetnek.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Példa használatieset 1: eSmart rendszereken – túlterhelés optimalizálása
[Intelligens rács](https://en.wikipedia.org/wiki/Smart_grid) fontos szerepet dinamikusan és folyamatosan optimalizálása és a módosítás fogyasztási módosítása. Rövid érvényességi idejű, hőmérsékleti ingadozásai (*pl.*további teljesítmény-alapú air feltétel vagy készítheti el fűtési) főként okozó módosítások energiafogyasztást is lehet hatással. Egy időben energiafogyasztást hosszú távú trendek is befolyásolja. Előfordulhat, hogy az érintett szezonalitás effektusok, nemzeti ünnepek, hosszú távú felhasználási NÖV és például fogyasztói index, olaj árát és GDP még economic tényezőket.

A használati eset [eSmart](http://www.esmartsystems.com/) , amely lehetővé teszi, hogy a innovációkká alakuljon egy túlterhelés helyzet, a minden megadott állomás a rács előrejelzésére felhőalapú megoldást üzembe szeretett volna. Mindenekelőtt eSmart megy végbe, azonosítása, amelyek valószínűleg a következő órán belül túlterhelés, így azonnal művelet sikerült tenni, így elkerülheti, és ez a helyzet megoldásához alállomások.

Egy pontos és gyors elvégzéséhez az előrejelzési szükséges prediktív három modell végrehajtása:
-   Hosszú távú modell, amely lehetővé teszi, hogy közben a következő néhány hetekben vagy hónapokban a minden állomás energiafogyasztást az előrejelzéshez
-   Rövid modell, amely lehetővé teszi, hogy egy adott állomás során a következő óra túlterhelés helyzet előrejelzése
-   Hőmérsékleti modell, ahol a jövőbeli hőmérsékleti felett több esetek előrejelzéshez

A hosszú távú modell célja, hogy a alállomások rangsorolása által az, hogy a Túlterhelés (adott azok power átviteli kapacitás), a következő hét vagy hónap során innovációkká alakuljon. Ez a rövid lista, amely egy rövid érvényességi idejű előrejelzésére beviteli szolgál alállomás létrehozását teszi lehetővé. A hosszú távú modell egy fontos előrejelző hőmérsékleti állapotában szükség van a folyamatosan több forgatókönyv hőmérsékleti előrejelzést készít, és a hosszú távú modellhez bevitt hírcsatorna őket. A rövid előrejelzés majd meghívott, mely állomás valószínű, hogy a következő órán keresztül túlterhelés előrejelzésére.

A hosszú és rövid távú modellek minden állomás egy külön-külön vannak telepítését. Ezek a modellek gyakorlati végrehajtását emiatt a teljes körű üzletifolyamat-tervező szükséges. Rövid magasabb előrejelzési pontosság eléréséhez finomabb modell minden nap órában kitűzött célja. Ezek a modellek óránként kell végrehajtani, és fejezze be az adatvégrehajtás válaszol, és ha szükséges, a megelőző műveletek végrehajtása elegendő időre néhány percen belül. A modellek gyűjtemény használatával időszakos átképzési a legfrissebb adatok naprakész legyen.

További információt a használati eset megtalálható [Itt](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Eset minősítési feltételek – Előfeltételek használata
A fő Cortana intelligencia erőssége hatékony képessége üzembe helyezéséhez és gépi tanulási oldalközpontú megoldások méretezni. Az előrejelzések párhuzamosan végrehajtott ezreselválasztó támogatásához szolgál. Automatikusan méretezheti változó felhasználási mintát alkalmazást. A megoldás ezért elsősorban a pontosságát és számítási teljesítményét. Például a vállalat is pontos energy igény szerint a következő órát, valamint a nap órát előrejelzési előállító érdekli. Azonban, hogy érdeklik kisebb, az igény szerinti miért van előre jelzett módon lehet, hogy a kérdés megválaszolásához (a modell magát fog gondoskodik arra, hogy).

Ezért fontos, hogy nem az összes használjon esetekben és üzleti problémák hatékony megoldhatók gépi tanulási használatával látja.

Cortana üzletiintelligencia- és gépi tanulási hatékonyan az egy adott üzleti probléma megoldását, az alábbi feltételek teljesülése esetén lehet:
-   A vállalati probléma kéz a **cserélendő** jellegű. A cserélendő használata eset példája egy segédprogram vállalatot, amely az egy adott állomás power terheltsége előrejelzésére során a következő óra szeretné. Kézzel elemzése és a korábbi igény szerinti vezetői rangsorolást lenne **leíró** jellegű, ezért kisebb alkalmazható.
-   Miután a szövegbevitel nem érhető el megteendő törlése az **elérési útját művelet** nem. Például a egy állomás során a következő óra túlterhelés előrejelzésére válthat egy adott állomás társított terhelés csökkentését és megelőzve esetleg túlterhelés megelőző műveletet.
-   Használati eset egy **tipikus probléma típusától függően** ilyen jelöli, hogy mikor megoldani azt is váltaniuk megoldási más hasonló esetekben használja.
-   Az ügyfél állíthat be **és mennyiségi célok** a megoldás sikeres végrehajtás bemutatására. Például a energy igény szerinti előrejelzés egy jó mennyiségi cél lenne a szükséges pontosság küszöbértékét (*például*engedélyezett legfeljebb 5 %-os hiba) vagy ha állomás előrejelzésére túlterhelés majd pontosság (igaz pozitív ráta), és visszahívási (igaz pozitív mértékét) egy megadott küszöbértékét feletti kell lennie. Ezek a célok a az ügyfél üzleti célok kell származnia.
-   Nem egyértelmű **integrációs forgatókönyvek** a vállalat üzleti munkafolyamathoz. Például az állomás betöltés előrejelzés a rács vezérlőközpont: lehetővé teszi a túlterhelés megelőzése tevékenységek integrálhatók.
-   Az ügyfél **adatait** szeretné használni a megfelelő minőséggel használati eset (lásd: további szakaszában a következő, **Az adatok minősége**, ez playbook) támogatásához kész rendelkezik.
-   Az ügyfél mérőórákat foglal magába, felhőalapú oldalközpontú adatok architektúra vagy **felhőalapú gépi tanulási**, többek között az Azure Machine Learning és más üzletiintelligencia-csomagja Cortana összetevők.
-   Az ügyfél **-végpont adatfolyam** létesíteni, hogy létesítményekhez hajlandó a kézbesítési adatok az a felhő folyamatos kombinálásával, és a program kész **üzemeltető** a megoldást.
-   **Erőforrások jelöl ki** ki fogja aktívan kapcsolni a kezdeti kísérleti végrehajtása során, hogy ismeretek és tulajdonjogát a megoldás sikeres befejezését követően az ügyfélnek vihetők át készen áll a az ügyfélnek.
-   Az ügyfél erőforrás **profi képzett adatok**, egy adatok tudósok lehetőleg kell lennie.

A fenti feltételek alapján használati eset minősítése nagyban javíthatja a használati eset sikerességéről, és létrehozni egy jó beachhead későbbi használatra esetek végrehajtásához.

### <a name="cloud-based-solutions"></a>Felhőalapú megoldások
Azure a Cortana üzletiintelligencia-csomagja integrált környezet, amely a felhőben. A megoldás üzembe helyezése a fejlett analitikai felhőalapú környezetben rendelkezik, jelentős előnyökkel jár a nagyvállalatok számára, és egy időben jelentheti a nagy módosítása a vállalatok, akik helyszíni informatikai megoldások továbbra is használható. A energy szektor belül nem egyértelmű tendenciát fokozatos áttelepítést, az a felhő műveletek. A trend szükségem kézzel az Ugrás a fejlesztés az intelligens rács együtt, **Trendek**a fent ismertetett módon. A playbook összpontosít az energy tartomány felhőalapú megoldást, mint fontos előnyeit és egyéb megfontolások egy felhőalapú megoldást üzembe helyezné ismertetik.

Valószínűleg a legfontosabb felhőalapú megoldást előnye a költség. Megoldásként él felhő rendszerbe összetevők nem tartozik előzetes költségek vagy ELÁBÉ (a termékek eladott) összetevőt költségek társítva. Ez azt jelenti, hogy nem kell a hardver, szoftverek és informatikai karbantartási fektetni, és ezért nincs jelentős üzleti kockázat csökkentése.

Egy másik fontos előnye felhőalapú megoldások befizetések költség szerkezete. Felhőalapú kiszolgálók számítások vagy tárolására is rendszerbe, és közvetlenül az-szükség szerint alapon méretezett. Ez a felhőalapú megoldást költség hatékonyság előnyeit jelenti.

Végezetül nem informatikai karbantartási vagy a jövőbeli infrastruktúra kidolgozásában kapcsolatos összes Ez a felhőalapú felületek része, nincs szükség. Olyan mértékben, hogy Cortana üzletiintelligencia-csomagja tartalmazza a legjobb osztály szolgáltatások, és annak útikalauzhoz változó továbbra is. Új funkciók, összetevők funkciók folyamatosan bevezetett és is alapkoncepciójára.

Olyan céget, akkor csak most kezdi az áttűnés az a felhő azt is nagyon ajánlása fokozatos megközelítés állapotba végrehajtása egy felhőalapú áttelepítési útikalauz is tárgyal. Úgy véli, hogy segédprogramok és vállalatok energy a tartományban, ez playbook tárgyalt használati eset jelenítik meg a felhőben prediktív analytics-megoldások piloting kiváló lehetőség.

#### <a name="business-case-justification-considerations"></a>Üzleti eset Címkefelirat kapcsolatos szempontok
Sok esetben az ügyfél amelyek érdekelhetik készít egy adott használatieset, amelyben egy felhőalapú megoldás és gépi tanulási is fontos összetevők üzleti indoklását. Nem kedvelik-e egy helyszíni megoldás felhőalapú megoldást, amíg az előzetes költség összetevő minimális és a költség összetevők többsége társított tényleges használatát. Amikor egy energy előrejelzés a Cortana üzletiintelligencia-csomagja megoldást üzembe helyezné, több szolgáltatások is egy egyetlen közös költség struktúra integrálva. Például adatbázisok (*például*SQL Azure) tárolásához az nyers adatokból is használható, és kattintson a tényleges előrejelzések Azure Machine Learning használatos az előrejelzési szolgáltatások tárolni. Ebben a példában a költség szerkezet foglalhatja tárolására és tranzakció alapú összetevőket.

Kézzel egy jól érthető értékének üzleti működő egy energy igény szerint az előrejelzés (hosszú vagy rövid kifejezés) kell rendelkeznie. Erre valójában fontos figyelmen kívül hagyja, minden egyes előrejelzési művelet üzleti értékét. Például pontosan power betöltés előrejelzés a következő 24 órát megakadályozhatja, hogy túltermelés vagy segít megakadályozni, hogy a rács túlterhelések és a naponta pénzügyi megtakarítási értelmez kell mennyiségi.

Az igény szerinti pénzügyi előnyeit kiszámításához egyszerű képleteket, előre jelzett megoldást jelenthet: ![egyszerű képlet kiszámításához az igény szerinti pénzügyi előnyeit előrejelzési megoldás](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Mivel a Cortana üzletiintelligencia-csomagja tartalmaz befizetések árak modell, nem ezt a képletet az összetevő fix költség felmerülése nincs szükség. Ez a képlet a naponta, a havi vagy éves alapján számítható ki.

Aktuális Cortana üzletiintelligencia-rendszerben, és Azure Machine Learning csomagok árak megtalálható [Itt](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Megoldás fejlesztési folyamata
Egy energy igény szerinti előrejelzés megoldás általában magában foglalja a 4 fázisok az, ami VÁLLALUNK fejlesztési ciklus felhőalapú technológiák és a Cortana üzletiintelligencia-programcsomag belül szolgáltatások használatára.

Ezt a az alábbi ábra mutatja:

![Intelligens rács ciklus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

A következő bekezdés ismerteti a 4 lépésben:

1.  **Adatgyűjtés** – bármely fejlett analitikai alapú megoldás támaszkodik adatok (lásd: az **Adatok ismertetése**). Kifejezetten amikor és a prediktív analytics előrejelzés, telefonszámokkal folyamatban lévő, dinamikus adatáramlást a. Esetén az előrejelzés igény szerinti energy, ezeket az adatokat közvetlenül az intelligens méter is lehet kifejezéskészletébe, vagy már összesítenie kell egy helyszíni adatbázist. Azt is más külső forrásból származó adatok, például az időjárási és hőmérsékleti támaszkodhat. A folyamatban lévő adatáramlást kell orchestrated, ütemezett és tárolja. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF) nem a fő workhorse végrehajtásának ezt a feladatot.
2.  **Modellezési** – pontos és megbízható energy az előrejelzések, egy kell kidolgozása (vonaton), és nagyszerű modell, hogy megkönnyíti a korábbi adatok felhasználása, és az első karaktertől a értelmes prediktív minták és az adatok karbantartása. A növő gyorsan már rendelkezik a a területen a gépi tanulási (Machine Learning) speciális algoritmusok rendszeresen fejlesztés alatt. Azure Machine Learning Studio, amely segít a csatlakozást a tapasztalt Machine Learning algoritmusok belül egy elvégzéséhez folyamata nagy felhasználói felületet nyújt. Munkafolyamat mutatja be, intuitív munkafolyamat-diagram, és a az adatok előkészítése, szolgáltatás kivonása, modellezési és modell kiértékelés tartalmaz. A felhasználó átteheti száz különböző modellek szereplő ebben a környezetben. Ez a szakasz végén egy adatok tudósok munka modell, amely a teljes mértékben kiértékelt, és készen áll a telepítéshez fog rendelkezni.

    A következő ábrán egy tipikus munkafolyamat ábrája:

    ![Munkafolyamat modellezése](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Telepítési** – kéz, a munkaidő-modellel a következő lépésként telepítési. Itt a modell teszi lehetővé, hogy a különböző felhasználási ügyfelektől az interneten keresztül egyidejűleg meghívott RESTful API webszolgáltatás alakul. Azure Machine Learning biztosítja az egyszerű üzembe helyezése a modell közvetlenül az Azure Machine Learning Studio egy kattintással a gombra. A teljes telepítési folyamatot történik a motorháztető alatt fülre. Ez a megoldás automatikusan felel meg a szükséges felhasználás is méretezhető.

4.  **Felhasználás** – a fázis, tudjanak VÁLLALUNK használatával az előrejelzési modell előrejelzések. A felhasználás is lehet alapú (*pl.*:, irányítópult) egy felhasználó alkalmazásból, illetve közvetlenül egy műveleti rendszer, például igény szerinti/nyújtásával terheléselosztás rendszer vagy a rács optimalizálási megoldást. Több használati eset is vezeti egyetlen modellből.

## <a name="data-understanding"></a>Adatok ismertetése
Után azt fedő egy energy igény szerint az előrejelzés megoldás üzleti szempontok (lásd: **Vállalati ismertetése**), akkor készen állnak adatok rész megvitatása. Bármely prediktív analytics megoldás megbízható adatok támaszkodik. Az előrejelzés igény szerinti energy, azt támaszkodhat különféle szintű engedélyeknek a korábbi felhasználási adatok. Korábbi adatokat nyers anyagokért szolgál. Azt fogja végezni, amelyben az adatok tudósok kiszámítására (más néven szolgáltatások), amely a modell, amely a lyncnek hoz létre a szükséges előrejelzések elhelyezendő azonosítja a óvatos elemzésre.

Ebben a szakaszban a többi hogy leírja a különböző lépéseket és megfontolandó szempontok ismertetése az adatokat, és hogyan használható formában hozhatja.

### <a name="the-model-development-cycle"></a>A modell fejlesztési ciklus
Jó modellek csak néhány ügyeljen előkészítése előrejelzéshez és tervezési előállító. A több lépést modellezése folyamat bontásban és egyszerre csak egy lépésben összpontosul jelentősen javíthatja a teljes folyamat eredményét.

Az alábbi ábra bemutatja, hogyan a modellezési folyamat sikerült bontásban kell több lépéseket:

![Modell fejlesztési ciklus](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Ahogy láthatja a ciklus hat lépésből áll:
-   A probléma kialakítása
-   Adatok bevitel és az adatok feltárása
-   Adatok előkészítése és szolgáltatás műszaki
-   Modellezési
-   Modell értékelése
-   Fejlesztési

Ebben a szakaszban a többi azt leírja a egyes lépések és az elemek minden lépésnél figyelembe.

### <a name="problem-formulation"></a>A probléma kialakításához
Azt is figyelembe kell venni bármely prediktív analytics megoldás alkalmazása előtt a egyik legfontosabb lépésként probléma kialakításában. Itt azt lenne a vállalati probléma átalakítás és felbontani, akkor az adott elemeket, amelyek adatokkal és modellezése technikákkal megoldható. Tanácsos a probléma megfogalmazásához formájában kérdések megválaszolásához szeretné azt. Íme néhány lehetséges kérdésre, amely lehet vonatkozó energy igény szerinti előrejelzés hatálya alá:
-   Mit tartalmaz egy egyéni állomás várható terhelését a következő óra vagy nap?
-   A nap mely időszakában a fog a rács tapasztalni csúcs igény szerint?
-   A rács a várt csúcs betöltés fenntartására valószínűleg hogyan van?
-   Mennyi power kell a power állomás készítése során a nap órát?

Kialakítása a fenti kérdések lehetővé teszi, hogy us szűkítheti az első a megfelelő adatokat, és a vállalati probléma kéznél teljesen igazított megoldást végrehajtásához. Ezenkívül azt majd állíthat be néhány fontosabb mértékek, amelyek lehetővé teszik velünk a modell teljesítményét ki szeretné számítani. Például hogyan pontos az előrejelzés legyen, és mi az a hiba, amely továbbra is elfogadható által a vállalati tartomány?

### <a name="data-sources"></a>Adatforrások
Modern intelligens rácsának adatok különböző részei és a rács összetevői gyűjti össze. Ezeket az adatokat a művelet számos tulajdonságát és a kihasználtság power rácsának jelöli. Az előrejelzés energy igény hatálya alá azt a vitafórum meg megfelelően a tényleges igény szerinti felhasználási adatforrások vannak korlátozása. Egy fontos forrást energiafogyasztást intelligens méter. A földgömb körül segédprogramok gyorsan telepít meg az intelligens méter a fogyasztói. Intelligens méter a tényleges energiafogyasztást rögzítése, és az adatokat a vállalat folyamatosan közvetítése. Gyűjtött adatokat, és vissza időközre rögzített, 1 órára 5 percenként kezdve küldött. Speciális intelligens méter programnyelven távolról ellenőrzése és a tényleges felhasználás belül a családban egyenleg. Intelligens állapotjelzője adatok az általában viszonylag megbízhatóak legyenek, és idő tartalmazza. Így az igény szerinti előrejelzés egy fontos összetevő. Állapotjelzője adatokat is összesíteni (összegzett) a rács topológia belül több szinten: *átalakító, állomás, régió és stb*. Akkor válassza ki, hogy az előrejelzési modell építése szükséges összesítési szintet. Például ha a vállalat szeretne minden egyes a rács alállomások jövőbeli betöltés előrejelzési majd összes méter adatok kell a minden egyes állomás összesíti, illetve az előrejelzési modell egy bemeneteként használt. Hivatkozunk intelligens méter belső adatforrásként.

Egyéb külső adatforrások megbízható energy igény szerinti előrejelzés is támaszkodik. Egy fontos energiafogyasztást kiható tényező az időjárási vagy pontosan a hőmérsékleti. Korábbi adatok külső hőmérsékleti és energiafogyasztást közötti erős korrelációs jeleníti meg. Meleg nyár nap alatt, a fogyasztói ellenőrizze a légkondicionálók és fűtési rendszerekben téli bekapcsolás során. A rács helyen korábbi átlaghőmérsékletűek megbízható forrásaként kulcsfontosságú, ezért. Ezenkívül még telefonszámokkal hőmérsékleti pontos előrejelzés egy előrejelző energiafogyasztást, mint.

Egyéb külső adatforrások segít a energy igény szerinti előrejelzési modellek létrehozása is. Hosszú távú előfordulhat, hogy az érintett környezet módosításait, gazdaságos indexek (*pl.*GDP), és másokkal. A dokumentum azt nem fogja tartalmazni ezek többi adatforrástól.

### <a name="data-structure"></a>Adatstruktúra
A szükséges adatforrások azonosítása, miután azt szeretné, annak érdekében, hogy gyűjtött nyers adatokból tartalmazza-e a megfelelő adatokat funkciókat. Megbízható igény szerinti előrejelzési modell összeállításához lenne szükség győződjön meg arról, hogy gyűjtött adatok elemekből adatok, amely segít a jövőbeli igény szerinti előrejelzésére. Íme néhány alapvető állapíthatók a nyers adatokból adatstruktúra (séma).

Sorok és oszlopok áll a nyers adatokból. Minden egyes mérés megfelelője egyetlen sornyi adatot. Az adatok minden egyes sorát több oszlopra (más néven szolgáltatások vagy mezők) tartalmazza.

1.  **Időbélyeggel** – az időbélyegző mező a tényleges időpontot, amikor a mértékegység lett felvéve jelöli. Meg kell felelnie a közös dátum/idő formátumok egyikére. Dátum és idő egyaránt kijelzők kell tartalmaznia. A legtöbb esetben nincs szükség a második szintű engedélyeknek eddig rögzítendő alkalommal. Fontos, hogy adja meg, amelyben az adatok rögzítését időzónát.
2.  **Állapotjelzője azonosító** – Ez a mező azonosítja a állapotjelzője vagy a mértékegység-eszközt. A kockák változó, és egy számjegyet és karakterek kombinációját is alkothatják.
3.  **Fogyasztási érték** – Ez az egy adott dátum/idő, a tényleges felhasználás. A felhasználás mérhető kilowattórában (kilowatt-hour), vagy bármely más előnyben részesített egységek. Fontos tudni, hogy milyen mértékegységet kell maradnia egységes összes szélességértékek az adatok között. Egyes esetekben felhasználás is megadni a több mint 3 power fázisok. Ebben az esetben lenne szükség összegyűjtése független felhasználási fázisait.
4.  **Hőmérsékleti** – a hőmérsékleti általában gyűjtött, független listában. Azonban kell kompatibilis a felhasználás adatait. Tartalmaznia kell egy időbélyeg fentebb ismertetett, amelyek lehetővé teszik, hogy szinkronizálja az a tényleges felhasználás adatait. A hőmérsékleti értéket a Celsius-fokban vagy Fahrenheit lehet megadni, de át az összes mérték maradjanak egységes.
5.  **Hely –** A hely mezőben általában arra a helyre, ahová a hőmérsékleti adatok összegyűjtött rendelve. Irányítószám számként, vagy a szélesség és hosszúság (lat/hosszú) formátumban is képviselteti.

Az alábbi táblázatokban mutat példát egy jó felhasználás és hőmérsékleti adatok formázása:

|**Dátum**|**Idő**|**Állapotjelzője azonosító**|**1 fázis:**|**2 fázis:**|**3 fázis:**|
|--------|--------|------------|-----------|-----------|-----------|
|7/1/2015|10:00:00|ABC1234     |7.0-s        |2.1        |5.3        |
|7/1/2015|10:00:01|ABC1234     |7.1.        |2.2        |a 4,3        |
|7/1/2015|10:00:02|ABC1234     |6.0 alkalmazásban        |2.1        |4.0-s        |

|**Dátum**|**Idő**|**Hely**|**Hőmérsékleti**|
|--------|--------|-------------|---------------|
|7/1/2015|10:00:00|11242        |24.4           |
|7/1/2015|10:00:01|11242        |24.4           |
|7/1/2015|10:00:02|11242        |24.5           |

Látható legyen a fent, mint ebben a példában a 3 power fázisok társított felhasználás 3 különböző értékeket tartalmaz. Figyeljen arra, hogy a dátum és idő mező neve, azonban azokat is kombinálni lehet egyetlen oszlopot. Ebben az esetben a hely oszlopban jeleníti meg az 5 jegyű irányítószám formátum és a hőmérsékleti Celsius fok formátumban.

### <a name="data-format"></a>Adatok formázása
Üzletiintelligencia-csomagja Cortana támogatniuk kell a legfontosabb adatok formátum, például CSV, TSV, JSON, *stb*. Fontos, hogy az adatformátum marad egységes a projekt teljes életciklusának.

### <a name="data-ingestion"></a>Adatok bevitel
Energy igény szerinti előrejelzés van folyamatosan és gyakran előre jelzett, mert azt gondoskodnia kell arról, hogy a nyers adatokból halad-e teli tónusú és megbízható adatok bevitel eljárással. A bevitel folyamat biztosítania kell, hogy a nyers adatokból érhető el az előrejelzési folyamatot a szükséges időt. Ez azt jelenti, hogy az adatok bevitel gyakoriság az előrejelzési gyakoriság nagyobbnak kell lennie.

Példa: Ha az igény szerinti megoldás előrejelzés a 8:00 de naponta új előrejelzés eredményezne, majd annak érdekében, hogy az összes van már teljesen keresztül a szervezetbe eddig ponthoz, amely során az elmúlt 24 óra összegyűjtötte az adatokat, és van még tartalmazzák az adatok utolsó óra szükség.

Ehhez a Cortana üzletiintelligencia-csomagja támogatja a egy megbízható adatok bevitel folyamat különböző lehetőségeket kínál. A további tárgyalja a dokumentum **telepítési** szakasza.

### <a name="data-quality"></a>Az adatok minősége
A megbízható és pontos igény szerint az előrejelzés elvégzéséhez szükséges nyers adatforrás néhány alapvető adatok minőség feltétel meg kell felelnie. Speciális statisztikai módszerek néhány lehetséges adatok minőségű probléma kompenzálja is használható, bár továbbra is szükséges ahhoz, hogy bizonyos alapadatok minőségű küszöbérték, amikor új adatok ingesting vannak metsző. Íme néhány, nyers adatokból minőséggel kapcsolatos megfontolások:
-   **Hiányzó érték** – a helyzet, ha adott mérési gyűjtése nem utal. Az egyszerű követelmény, hogy a hiányzó érték ráta nem lehet 10 %-nál nagyobb bármely adott időszakra. Jelenik meg, hogy egyetlen értéket hiányzik, jelezni kell egy előre definiált érték használatával (például: "9999"), és nem "0", amelyek érvényes érték.
-   **Mérési pontosság** – a tényleges érték fogyasztási vagy hőmérsékleti pontosan kell rögzíteni. Téves szélességértékek téves előrejelzések hoznak létre. A mérési hiba általában a true érték 1 %-nál kisebbnek kell lennie.
-   **Mérési idő** – szükség a tényleges mérték igaz időpontját viszonyított több, mint 10 másodperc, hogy az adatok a tényleges időbélyeg gyűjtött fog nem tér.
-   **Szinkronizálás** – több adatforrásból használatakor (*pl.*, fogyasztási és hőmérsékleti) azt kell annak biztosítására, hogy nincs idő szinkronizálási problémák közöttük is. Ez azt jelenti, hogy az összegyűjtött időbélyeg két független adatforrásokból származó közötti időkülönbség nem haladhatja meg a több, mint 10 másodperc.
-   **Időtartam** - **Adatok bevitel**, a fent ismertetett módon azt függnek egy megbízható adatok folyamat és bevitel folyamat. Szabályozhatja, hogy azt a gondoskodnia kell arról, hogy azt szabályozzák, az adatok késés. Ez a tényleges mérték megtett és az idő, amelynél a Cortana üzletiintelligencia-csomagja tárolóba töltött, és használatra kész közötti időkülönbség van megadva. Rövid terhelés előrejelzés a teljes időtartama nem lehet nagyobb, mint 30 percig. A hosszú távú betöltés előrejelzés a teljes időtartama nem lehet nagyobb, mint 1 nap.

### <a name="data-preparation-and-feature-engineering"></a>Adatok előkészítése és szolgáltatás műszaki
Miután a nyers adatokból lett j.leny (lásd: **Adatok bevitel**), és biztonságos módon tárolt, készen áll dolgozható fel. Az adatok előkészítése fázis alapvetően véve a nyers adatokból és átalakítása (átalakítása, átalakításával), a modellezési fázis űrlapba. Előfordulhat, hogy tartalmazó egyszerű műveletek, például a nyers adatokból oszlopban, mivel a tényleges mért értékkel, szabványos értékek, összetettebb műveletek, például az [időt visszamaradást](https://en.wikipedia.org/wiki/Lag_operator)és mások használatával. Az újonnan létrehozott adatoszlopok nevezik adatfunkciók, és a folyamaton, ezek generálni nevezik szolgáltatás műszaki parancsot. Ez a folyamat végére azt szeretné, hogy egy új adatkészletet, amely a nyers adatokból kinyert és modellezése használhatók. Ezeken kívül az adatok előkészítése fázis gondoskodik a hiányzó értékeket (lásd: **Az adatok minősége**), és azok kompenzálja kell. Bizonyos esetekben akkor is kellene annak érdekében, hogy ugyanazt a skálát jelennek meg a minden érték az adatok normalizálása.

Ebben a szakaszban látható az energiát szereplő közös adatok funkciók közül egyesek igény szerinti előrejelzési modellek.

**Idő alapú szolgáltatások:** Ezek a funkciók a dátum/időbélyeg adatok származik. Ezek kibontása és weblappá alakítva kockák szolgáltatásokat, például:
-   Idő a nap – Ez az az órát, a nap, amely megnyitja az értékek a 0 – 23
-   Napi hét – Ez a hét napját jelenti, és megnyitja az 1 értéket (vasárnap) és 7 (szombat)
-   Napi hónap – Ez a tényleges dátumnak, és nyerhetik 1 és 31
-   Hónap év – Ez a hónap helyén, és megnyitja az 1 értéket (január) és 12 (December)
-   Ha a hétvégi – Ez a tulajdonság bináris érték, amely a 0 értékek munkanapok száma vagy az 1-es a hétvége
-   Ünnepi – Ez a tulajdonság bináris érték, amely a 0 értékek rendszeres napi vagy 1-es egy munkaszüneti nap
-   Fourier-kifejezések – Fourier adatokra szerepelnek, súlyok, az időbélyegző származik, és a szezonalitás (ciklus) rögzítésére szolgálnak az adatok. Mivel előfordulhat, hogy az adatok van több évszak azt több Fourier kifejezéseket szükség lehet. Igény szerint értékek előfordulhat, például éves, heti és napi évszak/ciklus eredménye 3 Fourier kifejezéseket.

**Szolgáltatások független mértékegysége:** A független szolgáltatásai az adatok minden olyan elemeket, azt a modell kiszámítására használni szeretné. Itt azt kizárja a függő funkciót, ami előrejelzésére lenne szükség.
-   Eltérés szolgáltatás – ezek eltolt idő a tényleges igény szerinti értékeit. Eltérés 1 szolgáltatások például tartsa a az igény szerinti értéket az előző órában (feltételezve, hogy a adatainak óradíj) viszonyított az aktuális időbélyeget. Hasonlóképpen, azt felvétele eltérés 2, 3, időbeli eltérés *stb*. Eltérés használt funkciók tényleges kombinációja határozza meg fázisban modellezés a modell eredmények értékelése.
-   Hosszú távon trending – Ez a funkció az igény szerinti év között a lineáris NÖV jelöli.

**Függő szolgáltatás:** A függő funkció a adatoszlopot, amely azt szeretné, hogy a modell előrejelzésére. A [felügyelt gépi tanulási](https://en.wikipedia.org/wiki/Supervised_learning)először láthatja el ismeretekkel a függő funkcióival a modell (ez is nevezik címkék) szükség. Ezzel a modell megtudhatja, a mintázatok, a függő szolgáltatás társított adat. Az előrejelzés energy igény szerint a szokásos szeretnénk találnia a tényleges igény szerint, és ezért azt szeretné használni a függő szolgáltatás.

**Hiányzó értékek kezelése:** Az adatok előkészítésére fázisban lenne szükség a legjobb stratégiájának kidolgozása kezelje a hiányzó értékeket. Ez leginkább történik a különböző statisztikai [adatok imputálási módszerek](https://en.wikipedia.org/wiki/Imputation_(statistics))segítségével. Esetén az előrejelzés igény szerinti energy, azt a szokásos imputálására hiányzó értékeket az előző elérhető adatpontok mozgó átlag segítségével.

**Adatok normalizálás:** Adatok normalizálás egy átalakítási, például az előrejelzés igény szerint az összes numerikus adatokat vihet át hasonló skálával használt egy másik típusú. Ez általában növeli a modell pontosságát és pontosságát. Azt szeretné általában ehhez a tényleges érték elosztja a az adatok körét.
Ez lesz az eredeti értéket méretezni 1 és 1 közötti általában egy kisebb tartományba.

## <a name="modeling"></a>Modellezési
A modellezési fázis, ahol a konvertálás az adatok a modellbe történik. Az alapvető van ezt a folyamatot a speciális algoritmusok beolvasása a korábbi (oktatás) adatokat, a mintázatok kibontása és a modell összeállítása. Modell össze a modell nem használt új adatok előrejelzésére később használható.

Ha van még akkor is akkor később felhasználhatja új adatokat, amelyek tartalmazzák a szükséges szolgáltatások (X) van felépítve pontszám munka megbízható modellt. A pontozási folyamat láthatóvá válik a állandó modell (a oktatás fázis objektumot) használja, és a cél változó Ŷ helyén előrejelzésére.

### <a name="demand-forecasting-modeling-techniques"></a>Igény szerint technikákat modellezése előrejelzéshez
Az igény szerinti előrejelzés VÁLLALUNK esetében használni korábbi adatait, amelyek szerint van rendezve. Általában hivatkozunk, [idősort](https://en.wikipedia.org/wiki/Time_series)idő dimenzió tartalmazó adatok. Az idő sorozat modellezési célja, hogy meg időt kapcsolódó trendek, szezonalitás, automatikus-korrelációs (időbeli korrelációs), és azokat a modellbe megfogalmazásához.

Az utóbbi években speciális algoritmusok kidolgozott az idősoros előrejelzés igazodik, és előrejelzési pontosságának növelése érdekében. Röviden ölelik fel őket az alábbi néhány.

> [AZURE.NOTE] Ez a szakasz nem célja egy gépi tanulási és előrejelzési áttekintése, hanem inkább az igény szerinti előrejelzéshez gyakran használt technikákkal modellezési rövid felmérés használható. További információk és oktatási anyagok kapcsolatban az idősoros előrejelzés erősen ajánlott az online könyv [előrejelzési: elvek és a gyakorlathoz](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (mozgó átlag)**](https://www.otexts.org/fpp/6/2)
Mozgó átlag közül az első olyan technikák, az idősoros előrejelzés használt pedig továbbra is a legtöbb egyik gyakran használt technikák ma kezdve. Érdemes emellett alapját további speciális előrejelzés technikákat. A mozgó átlag azt is előrejelzés a következő adatpont által átlagolása azt jelzi, ahol K a mozgó átlag sorrendjének K legutóbbi pont fölé.

A mozgó átlag módszer az előrejelzés simítás hatása van, és ezért előfordulhat, hogy nem kezeli jól nagy illékonyság az adatok.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**Trendérték (exponenciális simítás)**](https://www.otexts.org/fpp/7/5)
Az Exponenciális simítást végző súlyozott átlag, a legutóbbi adatpontokra használja annak érdekében, hogy a következő adatpont előrejelzésére különböző módszerek a családi. A arról, hogy magasabb súlyok hozzárendelése újabb értékeket, és a régebbi mért értékeket ez súly fokozatosan csökkentése. Számos különböző módszerek az család, akkor az adatokat, például [Vilmos-telek jellemzők szezonális módszer](https://www.otexts.org/fpp/7/5)egy részüket foglalja szezonalitás kezelését.

Néhány módszer is az adatok a szezonalitást kiszámíthatja.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatikus regressziós integrált mozgó átlag)**](https://www.otexts.org/fpp/8)
Automatikus regressziós integrált mozgó átlag (ARIMA) gyakran használt előrejelzés idősorozatban módszerek egy másik család. A mozgó átlag automatikus-regressziós módot gyakorlatilag egyesít. Automatikus-regressziós módszerek használata a regressziós modellek előző sorozat időértékeket véve annak érdekében, hogy kiszámítása a következő esemény pontjára. ARIMA módszereket is alkalmazhat szülőlemez módszerek adatpontok közötti különbség kiszámítása és azok helyett az eredeti mért értéket használja. Végezetül ARIMA is alkalmazza a fent ismertetett mozgó átlag módszerek. Minden, az alábbi módszerek egyikét különböző módokon, ha, mit szerkezetek ARIMA módszerek a családi.

Trendérték és ARIMA sokan használják ma energy igény szerint az előrejelzés és sok más előrejelzési problémák esetén használható. Sok esetben ezek kombinált való közös nagyon pontos eredmények nyújtásához.

**Általános több regressziós** A legfontosabb modellezési megközelítés statisztika és gépi tanulási tartományban regressziós modellek lehet. Az idősorok környezetben a regressziós előre (*például*az igény szerinti) jövőbeli értékeket használjuk. A regressziós azt készíthet a kiszámítására lineáris kombinációi, és ismerje meg, ezeket kiszámítására súlyok (más néven együtthatók) a tanfolyamok során. A célja, hogy a regressziós egyenes, amely az előre jelzett érték fog előrejelzési kiszámítására. Módszerek a regressziós alkalmasak, ha a cél változó numerikus és ezért is elfér az idősoros előrejelzés elemre. A regressziós módszerek, többek között a rendkívül egyszerű regressziós modellek, például a [Lineáris regressziós](https://en.wikipedia.org/wiki/Linear_regression) és speciális is, például döntési fák, [Véletlen erdők](https://en.wikipedia.org/wiki/Random_forest), [Adagolás hálózatok](https://en.wikipedia.org/wiki/Artificial_neural_network)és csillapítja döntési fák széles köre van.

Hozhat létre, amely megegyezik a regressziós probléma előrejelzés energy igény szerint adja vissza us rugalmasságot biztosít az adatok szolgáltatások, amelyek kombinálhatók kiválasztása a tényleges igény szerinti idő adatsorok és a külső tényezők, például a hőmérsékleti sok. A kijelölt funkcióiról további információt a mérnöki (lásd: **adatok előkészítése és szolgáltatás mérnöki**) szakaszában a playbook funkciót tárgyalja.

A végrehajtás és energiaellátó igény szerinti előrejelzések próbaüzem telepítésének változat, az, hogy a regressziós speciális modellek az Azure Machine Learning használható általában a legjobb eredményt adja, és VÁLLALUNK van talált használni őket.

## <a name="model-evaluation"></a>Modell értékelése
Modell kiértékelés belül a **Modell fejlesztési ciklus**kritikus szerepkörbe tartalmaz. Ebben a lépésben megnézi az érvényesítése az adatmodelleket és a valós életben adatokkal teljesítményét. Modellezési lépés során azt használja a rendelkezésre álló adatok egy részének tanfolyamok a modellt. A kiértékelés fázisban azt, hogy az adatok tesztelje a modell többi. Gyakorlatilag azt jelenti, hogy azt is töltését mutató modell új adatokat van már szerkezetátalakítás és ugyanazokat a szolgáltatásokat, mint az oktatás adatkészlet tartalmazza. Az ellenőrzési folyamat során azt segítségével azonban a modell találnia a cél változó adja meg a rendelkezésre álló cél változó helyett. Gyakran hivatkozunk ezt a folyamatot, mint pontozási modell. Azt szeretné majd használja a cél igaz értéket, és összehasonlítani a becsült lehetőségekből. Itt célja mérésére, és minimalizálhatja az előrejelzési hiba, ami azt jelenti, a különbség az előrejelzések és az IGAZ értéket. A hiba mérték mennyiségi meghatározására nagyon fontos, mivel azt szeretné az adatmodell finomhangolása és ellenőrzése, hogy valójában csökken a hiba-e. Pontosabb beállításra a modell történik, amelyek a tanulási folyamatot model paraméterek módosításával vagy hozzáadásával és eltávolításával adatfunkciók (néven [Paraméterek takarítás](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Gyakorlatilag ez azt jelenti, hogy előfordulhat, hogy szükség találta közötti modellezés, a szolgáltatás mérnöki és modellezése a többször kiértékelés fázisok mindaddig, amíg képesek csökkentheti a hiba a szükséges szintre.

Fontos, hogy a szövegbevitel hiba soha nem lesz NULL hangsúlyt soha nincs is tökéletesen előrejelző minden eredménye modell. Azonban van bizonyos nagyságát, amely a vállalat által elfogadható hiba. Az ellenőrzési folyamat során azt szeretné győződjön meg arról, hogy a modell előrejelzési hiba szintre vagy annál nagyobb mértékben a vállalati hibatűrést szintet. Ezért fontos, hogy a **Probléma összetétel** fázisban ciklus elején szintet a megengedett hiba.

### <a name="typical-evaluation-techniques"></a>Tipikus értékelési eljárások
Az alábbi mely előrejelzése a hiba mérhető és mennyiségi különböző módon. Ebben a részben azt a vitafórum értékelése technikákat megfelelő idősorok és előrejelzés energy igény szerint az adott lesz kijelölve.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
Abszolút százalékos hibaüzenet jelent MAPE jelöli. A MAPE közötti különbség kiszámítása azt minden előrejelzett pont és az adott pont a tényleges érték. Azt majd számszerűsítése: a pont egy hiba, a különbség a tényleges érték viszonyított arányát kiszámításához. Az utolsó lépésénél, hogy átlagos ezeket az értékeket. A használt MAPE matematikai képlete a következő:

![A képlet MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*, ahol A<sub>t</sub> érték tényleges, F-<sub>t</sub> a becsült értéket, és n a az előrejelzési horizont.*

## <a name="deployment"></a>Telepítési
Miután hogy rögzített lefelé a modellezési fázis, és a modell teljesítmény érvényesített azt mappába érkeznek a telepítési fázis készen áll. Ebben a környezetben üzembe azt jelenti, hogy az ügyfelet a modell használhatnak futtatásával tényleges előrejelzések rajta bármikor nagyméretű engedélyezése. A telepítési fogalma kulcs az Azure Machine Learning óta fő célunk folyamatosan meghívása előrejelzések, nem pedig a csak a betekintést lekérését az adatokat. A telepítési fázis része hol azt engedélyezése a nagyméretű felhasználandó a modellt.

Energy igény szerinti előrejelzés keretén belül a célja, hogy meghívása a folyamatos és periodikus előrejelzések biztosítva, hogy Adatfrissítés érhető el a modellt, és, hogy az előre jelzett adatokat küldi el az igénybe ügyfélnek.

### <a name="web-services-deployment"></a>Web Services telepítése
Azure Machine Learning fő telepíthető építőelemként parancsot a webszolgáltatás. Ez a leghatékonyabb mód engedélyezése a felhőben prediktív modell felhasználás. A webszolgáltatás beágyazza a modellt, és foglalja össze a [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). Az API-t, az alábbi ábra szemlélteti bármilyen ügyfél-kód részeként használható.

![Azt a szolgáltatás telepítési és felhasználási](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Is láthatja, ahogy a webszolgáltatás telepítve van a Cortana üzletiintelligencia-Suite felhőben, és az elérhető REST API-végpontra hívható. Más típusú ügyfelek különböző tartományok egyidejű a szolgáltatás révén a webes API indítható el. A webszolgáltatás is méretezheti a támogatási hívások egyidejűleg ezer.

### <a name="a-typical-solution-architecture"></a>Egy tipikus megoldási
Előrejelzés megoldás, egy energy igény szerinti telepítésekor azt érdeklik, amely túl a szövegbevitel webszolgáltatás kerül, és lehetővé teszi a teljes adatfolyam-végpont-megoldást üzembe helyezné. Azt az új előrejelzés meghívása időben lenne szükség győződjön meg arról, hogy a modell dokumentum aktuális adatok funkcióival. Ez azt jelenti, hogy az újonnan összegyűjtött nyers adatokból folyamatosan keresztül a szervezetbe, feldolgozott, és meg a modell készült szükséges funkciójához átalakítva. Egy időben hogy szeretné elérhetővé teheti az előre jelzett adatok használata az ügyfelek más vége. Van egy példa adatok továbbításához ciklus (vagy adatok folyamat) az alábbi ábra szemlélteti:

![Az előrejelzési végpont adatfolyam Energy igény szerint](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Ezek a lépések, amelyek a energy igény szerinti előrejelzési ciklus részeként történik:
1.  A telepített adatok méter milliónyi folyamatosan generál power felhasználási adatok valós időben.
2.  Az adatok folyamatban van gyűjtött és feltöltött történő egy felhőalapú tárházba (*pl.*Azure Blob).
3.  Feldolgozása, mielőtt a nyers adatokból állomás vagy regionális szinten van összesíti a vállalat által meghatározott.
4.  Szolgáltatás feldolgozása (lásd: **adatok előkészítése és szolgáltatás feldolgozása**) majd kerül sor és elő a működéséhez szükséges adatmodell képzés vagy pontozási – a szolgáltatás beállítása adatok (*például*SQL Azure) egy adatbázisban vannak tárolva.
5.  Az újbóli képzés szolgáltatás meghívott arra, hogy újra megtanulják az előrejelzési modell –, hogy frissített verziójának a modellt, hogy a pontozási webszolgáltatás használható állandó.
6.  A pontozási webszolgáltatás, amely megfelel az előrejelzési gyakorisággal időközönként hivatkoznak.
7.  Az előre jelzett adatok az end felhasználás ügyfél által is elérhető adatbázisban vannak tárolva.
8.  A felhasználás ügyfél olvassa be az előrejelzések, érvényes, az a rács és fogyasztása meg megfelelően a szükséges használatieset.

Fontos, hogy ne feledje, hogy ez a teljes ciklus teljesen automatikus ütemezés szerint fut. A teljes üzletifolyamat-e adatok ciklus tervezési eszközök, például az [Azure Data Factory](http://azure.microsoft.com/services/data-factory/)használatával történik.

### <a name="end-to-end-deployment-architecture"></a>Végpont telepítési architektúra
Annak érdekében, hogy egy energy igény szerinti előrejelzési megoldás a Cortana intelligencia gyakorlatilag üzembe helyezése, győződjön meg arról, hogy a szükséges összetevőt létrehozott és megfelelően konfigurálva szükség.

A következő ábrán egy tipikus Cortana üzletiintelligencia-alapú architektúrája, amely, és az adatok, amelyek a fent ismertetett folyamat ciklus orchestrates szemlélteti:

![Végpont telepítési architektúra](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

További információt a összetevők, illetve a teljes architektúra olvassa el a Energy megoldás sablont.
