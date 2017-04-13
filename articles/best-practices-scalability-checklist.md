<properties
   pageTitle="Méretezhetőség feladatlista |} Microsoft Azure"
   description="Méretezhetőség feladatlista útmutatást az Azure Autoscaling Tervező kérdésére választ adhatnak."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="scalability-checklist"></a>Méretezhetőség ellenőrzőlista

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="service-design"></a>A tervező szolgáltatás
- **A terhelést a partíciót**. Tervezze meg a folyamat különálló és decomposable részei. Egyes részen méretének csökkentéséhez a szokásos szabályok a kérdések és az egyetlen felelősség szétválasztására követő közben. Az alkatrészek oly módon, hogy az egyes számítási egységek (például szerepkör vagy egy adatbázis-kiszolgáló) használata maximalizálja felosztandó lehetővé teszi. Is egyszerűbbé teszi Ha át kívánja méretezni, az alkalmazás az adott erőforrások példányok hozzáadásával. További tudnivalókért lásd: a [Szétválasztás útmutatást számítja ki](https://msdn.microsoft.com/library/dn589773.aspx).
- A **Méretezés-tervezés**. Méretezés lehetővé teszi az alkalmazások növelésével reagálni változó betöltése és csökkentése a szerepkörök, példányainak várakozási sorba, és más szolgáltatásokhoz használni. Jó helyen jár az alkalmazás meg kell terveznie ez szem előtt. Ha például az alkalmazás és a szolgáltatások használ állapot nélküli, e-mailként való példányokban kérések engedélyezése kell lennie. Ez is megakadályozza, hogy hozzáadása vagy eltávolítása meghatározott példányok negatív a felhasználókat érintő. Érdemes is alkalmazhat konfiguráció vagy a beállítások automatikus észlelése példányainak hozzáadása és eltávolítása, hogy a kód alkalmazásban végezheti el a szükséges útválasztás. Például webalkalmazás előfordulhat, hogy segítségével sorban várakozó halmazának ciklikus megközelítésben továbbítja kéréseket dolgozó szerepkörök futó háttér szolgáltatások. A webes alkalmazás észleli a módosításokat a sikeres kérelmeket irányítása és alkalmazásáról a terhelés a sorok száma kell lennie.
- **Egy egységként skála**. Tervezze meg további erőforrások NÖV igazodik. Az egyes erőforrásokhoz a méretezés korlátai felső, hogy, és lépjen túl az ezek a korlátok sharding vagy dekompozíciós használatával. Határozza meg a skála egységek számát tekintve erőforrások pontosan meghatározott csoportját a rendszer. Így alkalmazása méretezési műveletek könnyebbé, és jobban, hogy negatív hatással a korlátozások egy része a teljes rendszerben az erőforrások hiánya – az alkalmazás. Például hozzáadása x előfordulhat, hogy a webes és dolgozó szerepkörök száma y további sorok és z számát tároló fiókok kezelése a szerepkör által generált további terhelését. Egy időosztás egységet x is tartalmazhatnak, webes és dolgozó szerepkörök, _y_ sorban várakozó és _z_ tároló fiókok. Az alkalmazás, hogy könnyen méretezett, egy vagy több skála egységek hozzáadásával tervezése
- **Ne ügyfélaffinitás**. Ha lehetséges, győződjön meg arról, hogy az alkalmazás nem igénylő affinitás. Példányokban, így lehet továbbítani kérelmeket, és példányainak száma nem számít. Ezzel elkerülhető a terhelést tárolásához, beolvasása és fenntartása állapot adatait az egyes felhasználókhoz is.
- **Platform autoscaling szolgáltatások előnyeit**. A futtatási platform támogatja-autoscaling lehetőséget, ha Azure Automatikus méretezéssel, például inkább azt az egyéni vagy harmadik fél mechanizmusok kivéve, ha a beépített mechanizmusa nem teljesíti az igényeknek megfelelően alakíthatja. Ütemezett méretezési szabályok használatára, ahol erőforrások biztosítja a lehető érhetők el a induló késedelem nélkül, de reaktív autoscaling hozzáadása a szabályokat, szükség esetén alkalmazkodás az igény szerinti váratlan módosításokkal. A szolgáltatás felügyeleti API autoscaling műveletek autoscaling módosíthatja, és egyéni Számláló hozzáadása szabályok használhatja. További tudnivalókért lásd: az [Automatikus méretezés útmutatást](best-practices-auto-scaling.md).
- **Kiürítése intenzív Processzor/IO műveleteket, mint a háttérben futó feladatokat**. Ha egy szolgáltatási kérelmet szeretne futtatni, vagy jelentős erőforrások felvegye hosszú időt vesz várhatóan, kiürítése a kérés külön tevékenységhez feldolgozása. Dolgozó szerepkörök vagy a háttérben feladatok (attól függően, hogy a használat platform) segítségével hajtsa végre az alábbi műveleteket. Ez a beállítás lehetővé teszi, hogy a szolgáltatás továbbra is további kérelmeket, és továbbra is válaszol.  További tudnivalókért olvassa el a [háttér feladatok útmutatást](best-practices-background-jobs.md)című témakört.
- **Elosztás a terhelést a háttérben futó feladatokat**. Ahol sok háttérben futó feladatokat, vagy ha a feladatokat kell jelentős idő vagy erőforrásokat, húzza szét a munka különböző több számítási egységek (például dolgozó szerepkörök vagy háttér feladatok). Behelyezi olvassa el a a [Fogyasztói mintát Gyorskorcsolyázásban](https://msdn.microsoft.com/library/dn568101.aspx)című témakört.
- **Felé áthelyezése fontolja meg egy _megosztott semmi_ architektúra**. A megosztott semmi architektúra független, ellenállni kérelem (például megosztott szolgáltatások vagy tárhely) nélkül egyetlen pont csomópontok használja. Elméletben ilyen rendszer szinte végtelen időre szóló méretezheti. A teljes mértékben megosztott semmi megközelítést általában nem célszerű a legtöbb alkalmazással, miközben azt rendelkezhetnek jobb méretezhetőség a Tervező lehetőséget. Például, hogy a kiszolgálóoldali munkamenet-állapot használatát, ügyfélaffinitás, és az adatok szétválasztás Példák jó áthelyezése egy megosztott semmi architektúra felé.

## <a name="data-management"></a>Adatok kezelése

- **Használjon olyan adatokat szétválasztás**. Az adatok osztása több adatbázisokban és az adatbázis-kiszolgálón keresztül, illetve a tervezés, az alkalmazás használatához adattárolás szolgáltatások, amelyek tartalmaznak a szétválasztás átlátszó (például az Azure SQL-adatbázis rugalmas adatbázis és Azure táblatárolóhoz). Ezt a megközelítést teljesítményét és könnyebben méretezés engedélyezése segítséget. Vannak más szétválasztás technikák, például a vízszintes, függőleges, és funkcionális. Ezek kombinációját is használhatja, a nagyobb a lekérdezési teljesítmény, egyszerűbb méretezhetőség, rugalmasabb management jobb elérhetőség maximális kedvezménye eléréséhez, és a megfelelő tárolóját, hogy az adatok tartalmazni fogja, hogy milyen típusú. Is akkor fontolja meg másik típusú adattárhoz különböző típusú adatokat, mennyire optimalizált azok az adatok adott típusú alapján kiválasztása. Ez kiterjedhet használata táblatároló, dokumentum adatbázis vagy egy oszlop-család adatok mentése, helyett, vagy valamint, relációs adatbázisból. További tudnivalókért lásd: az [adatok particionáló útmutatást](best-practices-data-partitioning.md).
- **Az esetleges konzisztencia látványtervet**. Esetleges konzisztencia méretezhetőség javítja a csökkentése, és eltávolításával több üzlet keresztül particionálva kapcsolódó adatok szinkronizálása szükséges időt. A költség akkor, hogy adatokat nem mindig egységes olvasható legyen, és néhány írási műveletek okozhat ütközéseket. Esetleges konzisztencia ideális azokról a helyzetekről, ahol ugyanazzal az adattal olvassa el a gyakori csak ritkán írt. További tudnivalókért lásd: az [Adatok konzisztencia alapozó](https://msdn.microsoft.com/library/dn589800.aspx).
- **Kicsinyítés chatty kapcsolati összetevők és -szolgáltatások között**. Elkerülésére, amelyben az alkalmazás több hívásokat szolgáltatáshoz szükséges kapcsolati tervezése (, amelyek ad vissza egy kis mennyiségű adattal), nem pedig annak a visszatérési adatok egyetlen hívásba. Ha lehetséges, összevonás több kapcsolódó műveletek az egyetlen összehívásba, ha a hívást, ha a szolgáltatás vagy, amelynek észrevehető késés összetevőt. Ezzel megkönnyíti a teljesítmény figyelését és optimalizálása összetett műveletek. Adatbázisok tárolt eljárások segítségével beágyazására komplex logikai, és a kerekítés utakat és erőforrás-zárolni szeretné a számának csökkentése.
- **Sorok szeretné simítani, a nagy sebesség adatot ír a betöltés használja**. Az igény szerinti egy szolgáltatás túlfeszültség elborít szolgáltatás és a escalating hibák okozhatnak. Ennek elkerülése érdekében fontolja meg a [terhelést a simítás mintát várólista-alapú](https://msdn.microsoft.com/library/dn589783.aspx)végrehajtása. Működik-e egy pufferelési tevékenység és szolgáltatás, amely azt meghívása között várólista használja. Ez lehet sima időnként nehéz terhelések sikertelen lesz a szolgáltatás vagy a tevékenység időtúllépést okoz egyébként okozó.
- **Kis méret a betöltés adatok áruházból**. Az adatokat tároló gyakran egy feldolgozás szűk, költséges erőforrás, és gyakran nem könnyen méretezése. Ha lehetséges, logika (például az XML-dokumentumokból vagy JSON objektumok feldolgozása) eltávolítása az adatok áruházból, és végezze el a feldolgozás, az alkalmazáson belül. Például helyett átadása XML az adatbázis (eltérő tárolására átlátszó karakterláncként), szerializálni vagy deszerializálni az alkalmazási réteg belül az XML és át egy űrlapot, amely hasonlít az adatok Store natív. A szokásos jelentősen megkönnyíti, ha át kívánja méretezni, mint az adatok mentése, az alkalmazás, végezze el az alkalmazáson belül a lehető számítási igényű feldolgozása annyi kell megpróbálja.
- A **kis méretre az adatok mennyisége beolvasott**. Lekérés csak azokat az adatokat tartalmazó oszlopok és a feltételek használatával jelölje ki a sorok van szüksége. Ellenőrizze a táblázat érték paraméterek használata és a megfelelő elkülönítési fokot. Használata szerkezetek például entitás címkék elkerülése érdekében a retrieving szükségtelenül az adatokat.
- **Újrahasznosítását a gyorsítótárat használni**. Használja a gyorsítótár-lehetőség csökkentheti a betöltés az erőforrások és a szolgáltatások készítése és továbbítja az adatokat. Gyorsítótár általában alkalmas, amely általában viszonylag statikus, vagy jelentős feldolgozás beszerzése igénylő adatokhoz. Gyorsítótár történjen minden szintjén adott esetben az alkalmazás, beleértve az adatokat az access és a felhasználói felület generációs rétegekre. További tudnivalókért lásd: a [Gyorsítótár útmutatást](best-practices-caching.md).
- **Adatok NÖV és az adatmegőrzési kezelni**. Az alkalmazás által tárolt adatok mennyiségét megnő idővel. Ez a legjobb exponenciális tárolási költségek növeli, és az adatok elérésekor növeli a késés – érintő alkalmazás átviteli és a teljesítmény. Rendszeres időközönként archiválni a régi adatok, amely már nem érhető el egy része, vagy ritkán használt adatok áthelyezése a hosszú távú tárolók további költség hatékony, akkor is, ha az access késés magasabb is lehet.
- **Optimalizálása adatok átvitele (DTOs) használó objektumok egy hatékony bináris formátum**. DTOs sokszor át a rétegeket, az alkalmazás között. A Méret minimalizálása csökkenti az erőforrások és a hálózat terhelését. Az adatok konvertálása szükséges formátumú minden olyan helyen, ahol használják a terhelést a megtakarítási azonban egyenleg. Fogadja el, amely tartalmazza a maximális interoperability ahhoz, hogy egy összetevő újrafelhasználás formátumot.
- **Gyorsítótár-vezérlésének**. Tervezze meg és állítsa be az alkalmazás weblapkimeneti gyorsítótárazás beállításával vagy részlet, ha lehetséges, terhelését minimalizálásához gyorsítótárazás.
- **Ügyféloldali gyorsítótárazás engedélyezése**. Webalkalmazások engedélyeznie kell a gyorsítótár beállításai a tartalmat, hogy gyorsítótárba helyezhető. Ez gyakran alapértelmezés szerint nincs engedélyezve. Állítsa be a kiszolgáló a megfelelő gyorsítótár gyorsítótárazás a proxykiszolgálók és az ügyfelek a tartalom engedélyezése beállítás a fejlécek előadásához.
- **Használata Azure blob-tárolóhoz és az Azure tartalom kézbesítési hálózati csökkentheti a betöltés az alkalmazást**. Fontolja meg, hogy statikus vagy viszonylag statikus nyilvános tartalmat, például képek, az erőforrások, a parancsprogramokat és a stíluslapok, tárolása blob-tárolóhoz. Ezt a megközelítést enyhíti az alkalmazását a betöltés dinamikusan létrehozása a tartalom egyes kérelme okozza. Ezenkívül fontolja meg inkább a tartalom kézbesítési hálózati gyorsítótár-e a tartalmat, és ügyfelek továbbítsa azt. A tartalom kézbesítési hálózaton keresztül is teljesítmény javítása az ügyfél, mert a tartalom a földrajzilag legközelebb adatközponthoz, amely tartalmazza a tartalom kézbesítési hálózati gyorsítótárat a rendszer kézbesítse. További tudnivalókért lásd: a [Tartalom kézbesítési hálózati útmutatást](best-practices-cdn.md).
- **Optimalizálás és finomításához SQL-lekérdezések és az indexek**. Néhány T-SQL-utasítások vagy elem hatással lehet a teljesítmény, amely a kódot a tárolt eljárás optimalizálása csökkenthető. Kerülje például **datetime** típusú konvertálása egy **varchar** **konstans időértékké** összehasonlítandó előtt. Dátum/idő összehasonlító függvények használja. Hiányoznak a megfelelő indexek is is csökkentheti a lekérdezés végrehajtása. Ha egy objektum relációs megfeleltetés keretrendszer használja, ismerje meg működéséről, és hogyan befolyásolhatják az adat-hozzáférési réteg teljesítményét. További tudnivalókért olvassa el a [Lekérdezés beállítása](https://technet.microsoft.com/library/ms176005.aspx)című témakört.
- **Fontolja meg, vonja vissza a normalizálás**. Adatok normalizálás segít a párhuzamos és ellentmondás elkerülése érdekében. Több indexek fenntartása, keresésekor a hivatkozási integritás megőrzése, kis mennyiségű adat a többszörös hozzáféréshez elvégzéséhez és az adatok elhelyezkedését táblák illesztése ró az általános teljesítményt befolyásoló. Fontolja meg, ha néhány további tárterületet mennyiségi és a párhuzamos elfogadható a betöltés az adatokat tároló csökkentése érdekében. Fontolja meg a is, ha az alkalmazás magát (Ez általában egyszerűbb, ha át kívánja méretezni) is lehet hivatkozni a tevékenységek kezelése a hivatkozási integritás megőrzése érdekében csökkentse a terhelést a adatokat tároló például átvétele. További tudnivalókért lásd: az [adatok particionáló útmutatást](https://github.com/mspnp/azure-guidance/blob/master/Data%20partitioning.md).

## <a name="service-implementation"></a>Szolgáltatás végrehajtása
- **Aszinkron hívások használatát**. Aszinkron kód lehetőség esetén használhatnak elérésekor erőforrások vagy I/O vagy hálózati sávszélesség, vagy ha függvényében korlátozva lehet szolgáltatások észrevehető késés, hogy a jövőben elkerülhesse zárolni szeretné a hívási szál. Aszinkron művelet végrehajtásához használja a [Feladatalapú aszinkron mintát (KOPPINTVA)](https://msdn.microsoft.com/library/hh873175.aspx).
- **Kerülje a források zárolása és optimista megközelítés használja helyette**. Soha nem erőforrások, például tárhely való hozzáférés zárolása és más szolgáltatások, amelyek észrevehető késés, mert ez teljesítményproblémákat elsődleges oka. Mindig használata optimista megközelítés egyidejű műveletek, például írása tárhely kezelése. A tároló réteg funkciók használatával kezelheti a ütközéseket. Az elosztott alkalmazásokban adatok csak lehet ahányat egységes.
- **Nagy késést, kis sávszélességű hálózatok fölé erősen tömöríthető adatok tömörítése**. A legtöbb esetben a webes alkalmazásokban a legnagyobb mennyiségű adat, az alkalmazás által létrehozott és a hálózaton keresztül átadott található ügyfél kérések HTTP válaszokat. HTTP-tömörítés csökkentheti a jelentősen, különösen a statikus tartalommá. Költség, valamint a terhelést a hálózaton csökkentése ezzel csökkenthető a abban az esetben, ha tömöríti a dinamikus tartalom alkalmazása egy fractionally magasabb betöltése a kiszolgálón. Más, általános környezetekben adatok tömörítés csökkentheti az átvitt adatok mennyisége és összezárása átviteli idő és a költségeket, de a tömörítése és folyamatok terhelést merülnek fel. Ilyen tömörítés csak használandó a teljesítmény bizonyítható nyereség esetén. A többi szerializálási módszert, például JSON vagy bináris kódolások, előfordulhat, hogy tartalom méretének csökkentése közben kevesebb gyakorló teljesítmény elérése érdekében mivel XML valószínűleg növeli
- **Kis méret kapcsolatok és erőforrások használatban lévő időpontját**. Kapcsolatok és a csak az erőforrások karbantartása mindaddig, amíg kell használnia őket. Például nyissa meg a kapcsolatok minél, és teszi lehetővé, vissza kell a kapcsolat kvótáját minél korábban beállítást. Minél később lehetséges szerezheti be az erőforrások, és azokról a minél korábban beállítást.
- A **kis méretre a kapcsolatok száma szükséges**. Szolgáltatási kapcsolaton felvegye erőforrásokat. Megadhatja, hogy szükség, és győződjön meg arról, hogy a meglévő kapcsolatok minden lehetséges esetben fel újra. Ha például után hitelesítése, az megszemélyesítési esetben egy adott identitással kód futtatásához. Ez segít, hogy kihasználhassa a kapcsolat készlet újbóli felhasználása diagramsablonok a kapcsolatokat.

    > [AZURE.NOTE]: APIs for some services automatically reuse connections, provided service-specific guidelines are followed. It's important that you understand the conditions that enable connection reuse for each service that your application uses.

- A **hálózati optimalizálja kötegekben-összehívást**. Például küldése kötegekben üzenetek olvasása, amikor egy várólista és ténykedés több olvasása és írása egy köteg tárhely és a gyorsítótár elérésekor. A szolgáltatások és az adatok tárolók hatékonyságának maximalizálása hívások száma a hálózaton keresztül csökkentésével segítségével.
- **Ne a követelmény, hogy a kiszolgálóoldali munkamenet-állapot tárolási** lehetőség. Kiszolgálóoldali munkamenet állapot-kezelés általában csak ügyfélaffinitás (tehát minden kérés van, az azonos server-példányt továbbítás), amely hatással van a rendszer lehetőségét, ha át kívánja méretezni. Ideális esetben tervezheti meg ügyfelek számára, hogy a kiszolgálókat, amelyeket használnak részletez állapot nélküli legyen. Jó helyen jár Ha az alkalmazás kell kezelése munkamenetet állapot, tárolása bizalmas adatokat vagy nagy mennyiségű adattal ügyfél elosztott kiszolgálóoldali gyorsítótárban, az alkalmazás az összes előfordulását is rendelkeznek hozzáféréssel.
- **Optimalizálás a táblázat tároló sémák**. Tábla tárolja, hogy az a tábla- és oszlop nevét át, és minden egyes lekérdezés például Azure táblatárolóhoz feldolgozása használatakor fontolja rövidebb nevek csökkentse a terhelést. Azonban nem feláldozhat olvashatósági vagy kezelhetőség túl kompakt nevük segítségével.
- **A tevékenység párhuzamos Library (TPL) aszinkron műveletek elvégzéséhez használja**. A TPL egyszerűen aszinkron kódírás, amely lehet/O – kötött műveleteket hajt végre. Egy adott szinkronizálási környezetben fenntartása függőségét kiküszöbölése érdekében lehetőség szerint _ConfigureAwait(false)_ használata Ez csökkenti a csomagváltás szál-kölcsönös kizárás száma.
- **A telepítés során, vagy alkalmazás indításakor létrehozása az erőforrás-függőségek**. Erőforrás megléte vizsgálat, majd hozza létre az erőforrás, ha még nem létezik módszerek ismételt hívások elkerüléséhez. (Például _CloudTable.CreateIfNotExists_ és _CloudQueue.CreateIfNotExists_ az Azure tároló ügyfél tárban módszerek kövesse ezt a minta). Ezeket a módszereket is írhat elő jelentős terhelést, ha az azok vételét tároló tábla vagy tároló várólista minden hozzáférést előtt. Ehelyett:
 - A szükséges erőforrások létrehozása, amikor az alkalmazás telepítve van, vagy azt először indítja el (a az egyes erőforrásokhoz egy webhely vagy dolgozó szerepkör indítási kódban egyetlen hívást kezdeményez _CreateIfNotExists_ elfogadható). Azonban lehet, hogy miként kezelje a kivételek felmerülő, ha a kód kísérel meg hozzáférni egy nem létező erőforrás. Ezekben az esetekben kell jelentkezzen be a kivételt, és esetleg a felhasználó operátor, hogy egy erőforrás hiányzik.
 - Bizonyos körülmények között célszerű lehet a kivétel kezelése kód részeként a hiányzó erőforrás létrehozásához. De lehet, hogy az erőforrás nem jelenléte tájékoztató programozási hiba (például hibás erőforrás nevét), illetve bizonyos egyéb infrastruktúra szintű probléma, el kell fogadnia körültekintően ezt a módszert.
- **Hozza létre, keretek használatát**. Gondosan válassza a az API-k és keretek kisméretűvé erőforrás-kihasználtság, végrehajtás idő és az alkalmazás általános terheltsége használatával. Például az alkalmazások helyigénye csökkentheti és végrehajtási sebességének növeléséhez webes API segítségével kezeli a szolgáltatási kérelmeket, de nem feltétlenül alkalmas speciális olyan esetek, ahol a további lehetőségeket a Windows Communication Foundation szükség.
- **Fontolja meg a kis méretre szolgáltatás fiókok számát**. Ha például az access erőforrás egy meghatározott fiókot vagy kapcsolatok korlátozzák, vagy végezzen szolgáltatások jobb, ahol kevesebb kapcsolatok karbantartott. Ezt a megközelítést közös szolgáltatások, például az adatbázisok, de azt befolyásolhatja pontosan a műveleteket, mert a megszemélyesítés, az eredeti felhasználó jelölőnégyzetét.
- **Teljesítmény adatainak összegyűjtése és a betöltés tesztelés végrehajtása** a fejlesztés próba eljárások, valamint a végleges annak érdekében, hogy az alkalmazás végrehajt, és szükség szerint értékekhez előtt részeként során. Ez a tesztelés jelenhet meg a gyártás platform, és az azonos típusú hardver azonos típusú, és és a felhasználói adatok mennyiségét betöltése, ahogy azt az gyártás fog megjelenni. További tudnivalókért olvassa el a [egy felhőalapú szolgáltatásba teljesítményét tesztelve](vs-azure-tools-performance-profiling-cloud-services.md)című témakört.