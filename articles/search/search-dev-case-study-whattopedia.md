<properties 
    pageTitle="Azure keresési Fejlesztőeszközök esettanulmány: Hogyan WhatToPedia, a Microsoft Azure-infomedia portál épített |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás" 
    description="Megtudhatja, hogyan hozhat létre egy információk portál és -metaadatok keresőmotor Azure keresés a felhőben tárolt keresési szolgáltatásának fejlesztők számára." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Azure keresési Fejlesztőeszközök esettanulmány

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Hogyan [WhatToPedia.com](http://whattopedia.com/) beépített a Microsoft Azure-infomedia portál

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">A nagy arról</font> 


A lényege az adatok portált, amelyek segítségével kapcsolatba lépni a nagyon fontos, a keresés hatóköre változat, hogyan utazási portálokról egyezés turisták felfelé a szállodák, éttermek és szórakoztató uncharted területén a hasonló keresztül kereskedők vásárlóktól össze. 

A portál envision azt egy adott piacon viszonteladótól adatok fölé egy rendkívül kiváló minőségű keresőmoduljának olvasása közben, így keresse meg a helyet, és egyéb eszközök alapján tárolja a viszonteladótól vásárlóktól biztosít. Azt rendezendő a keresőmotor, egy kezdeti adatkészlet, de mélyebb érték időbeli viszonteladótól előfizetők számára az üzleti adatait-bejegyzést közzétevő segítségével épül. Promóciók, új termékek, népszerű márka, házon belüli speciális szolgáltatások – – az összes adatokat felveszi az value webhelyünkön ábrázolnak. Ezeket az adatokat önálló jelentett és a keresési corpus beépül, miután mint előfizető regisztrál az viszonteladótól. Hirdetések, valamint a előfizetés modell adja meg a bevétel adatfolyam az új vállalati.

Keresés a meghatározó felhasználói beavatkozás tiszta felhő platformon modell lesz. Méretezés és a kis-költségek alkalmazásában szinte minden, akkor hajtsa végre, az adatforrás-vezérlő, a portál funkcióit lesz, egy online szolgáltatáson keresztül. Használatával olyan keresőprogram, amely a szükséges szolgáltatásokat nyújt beépített, létrehozhatunk egy keresőalkalmazás gyorsan, anélkül, hogy létrehozása és kezelése egy keresési motor ragozott formáival.

## <a name="what-we-built"></a>Úgy alakítottuk

WhatToPedia indítási infomedia vállalata. Úgy alakítottuk ki [WhatToPedia.com](http://whattopedia.com/) – - jelenleg a próba-piacra Észak-Európában éles 2015 február 2 dátummal. Az ügyfél alapja elsősorban ki kell egy könnyen kezeléséhez és karbantartásához elérhető online jelenléti tégla mozsárban üzletek.

Remek online keresőmoduljának keresztül vásárlóktól vonzhat webhelyére, alapján eredményt szolgáló lehetőségekről a város vagy a helyek, a márka tárolni a nevek, illetve tárolhatja típusú, akkor a feladatot. Egy továbbgyűrűző hatással vannak, valamint vásárlóktól rendelkezik motiváló kereskedők portál webhelyünkön előfizetéséhez. Az előfizetések olyan díjköteles, elérhető kamatlábbal.

 ![][7] 

A regisztráció után előfizetéshez viszonteladótól elsőbbrendű a meglévő profilját (kezdetben által létrehozott us vásárolt adatokból), akkor promóciók, kiemelt márka vagy hirdetmények további adatainak frissítése. Házon belüli funkciókat, például, beszélt nyelveket bemutató fogadja el a pénznemek, adómentes vásárlás lehet önálló jelentett szeretné jobban vonzhat webhelyére vásárlóktól, akik e eszközök keres.

## <a name="who-we-are"></a>Kik vagyunk

A nevem Balázs Segato (Microsoft Consulting), és lehet Lukács Boelling, WhatToPedia a megoldás tervezésekor az érdeklődő Fejlesztőeszközök együttműködik. 

WhatToPedia, egy kezdeti, Svédország, ahol a 60 000 kereskedők táblázatparancsok tégla mozsárban KKV (kicsi, közepes és méretű nagyvállalatoknak) az új portál üzleti marketing próba. Tudjuk, hogy egy személy Európában vásárlás speaks több nyelv, és több pénznemek végzi, mert azt, hogy beleférjenek egy többnyelvű shopper megoldások hozhat létre. Hogy szükség, és található, olyan keresőprogram, amely támogatja a többnyelvű követelmények Azure keresés.

Azure keresés nem mérkőzés szavakat-betöltőnyílásában beállítása a projekthez. Azure keresési állásáról előtt azt felhasználási jelentős energy dolgozik, a saját keresőmotor tudnivalóit kinks keresztül. Azure keresési problémákat, mint egy online szolgáltatás a legnagyobb érték technikai és felügyeleti eltávolítja a megoldást, amely szólnak a piaci gyorsabban és egy nagyobb teljesítményű keresési élményt első.  

## <a name="how-we-did-it"></a>Hogyan azt kerültek

A látás volt, hogy csak a felhőszolgáltatások alapján egy teljes infrastruktúra összeállítása. Microsoft választotta a stratégiai platform, mert a szolgáltató, amely az ehhez szükséges ajánlott szolgáltatások (együttműködési és is fejlesztési), igény szerint, és elérhető árak méretezni.
 
### <a name="high-level-components"></a>Magas szintű összetevők

Úgy alakítottuk ki egy vállalati, nem csak egy webhelyen. Eszközök és-alkalmazások teljes körű támogatása a teljes munkamennyiség szükséges. Visual Studio és a Visual Studio Team Services fejlesztése, az adatforrás-vezérlő Team Foundation szolgáltatás (TFS) Online és a scrum kezelésére, kommunikáció és együttműködés az Office 365 és a természetesen Microsoft Azure az összes webhely kapcsolatos műveletek és tárhely elfogadott azt. A Visual Studióban az IDE megadott közvetlen kiépítési az Azure-integrációval TFS online egy további hatékonyság erősítése kezeléséről.

Az alábbi ábrán a WhatToPedia infrastruktúra használt magas szintű alkatrészek mutatja be.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Microsoft Azure használata

Az előző diagram kereteibe zöld megnézve jelenik meg, hogy a WhatToPedia megoldást épül, az alábbi szolgáltatások:

- [Azure keresés](https://azure.microsoft.com/services/search/)
- [Azure webhelyek MVC 4 használatával](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs ütemezett tevékenységek](../app-service-web/websites-webjobs-resources.md)
- [Azure SQL-adatbázis](https://azure.microsoft.com/services/sql-database/)
- [Azure BLOB-tárolóhoz](https://azure.microsoft.com/services/storage/)
- [SendGrid e-mailek kézbesítési](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

A megoldás a nagyon szívecskés az adatok és a keresés. A folyamat befejezése ügyfél a viszonteladói szolgáltatótól kapott adatok bemutat:

  ![][9]

Elsődleges adattárolás pedig a viszonteladói könyvelési adatokról Azure SQL-adatbázisban. Ez a kezdeti adatkészlet, valamint az adott idő alatt hozzáadott viszonteladótól-specifikus adatok áll. A keresési corpus keresés Azure SQL-adatbázisból a frissítések közzétételi programmal mutatjuk be egy Azure WebJob.

### <a name="presentation-layer"></a>Bemutató réteg

A portál nem MVC 4 és a [Twitter betöltő](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29)szerepelni fog az Azure webhelyén. Mivel ez biztosít a sok tisztább megközelítés HTML mint ASP.NET űrlapalapú fejlesztési választottunk MVC. Twitter betöltő elkerülése érdekében, hogy több eszközzel-alkalmazások létrehozása és több mobil platformokon karbantartása, eszközök és a platformokon támogatása választott.

### <a name="authentication-permissions-and-sensitive-data"></a>Hitelesítés, az engedélyek és a bizalmas adatokat

Vásárlóktól névtelenül keresse meg a webhelyet. Ilyen vásárlóktól nincs bejelentkezési követelményeknek, sem azt tárolja az bármely fogyasztói adatokat. 

Kereskedők különböző írás. Ebben az esetben tároljuk nyilvános felhasználóiprofil-adatok, a számlázási információkat és a multimédiás tartalom kattintva jelenítse meg a webhelyen, amelyet. Minden viszonteladótól ki azt a webhelyet, előfizet egy felhasználónak bejelentkezés, a felhasználói frissítések végez az előfizetői profil előtt hitelesíti kaphat.  Azt biztonságos tárolása az összes előfizető adat Azure SQL-adatbázis és Azure BLOB-tárolóban lévő.
Azt választotta,-hitelesítés modell .NET űrlapalapú hitelesítés alapján. Az az egyszerűség; eljárás választottunk a szerepkörök, felhasználói felület és egyéb felesleges szolgáltatásokat, amelyek más módszer beépített nem szükséges. 

Győződjön meg arról, hogy kereskedők csak a hozzájuk tartozó adatokat látja, hogy minden később használt összes viszonteladótól azonosítója írási és olvasási érintő viszonteladótól-specifikus műveletek viszonteladótól létrehozott. Ez a módszer a talált, hogy megváltozott nincs szükség egyes kereskedők adatbázis engedélyeket. Az összes kereskedők egy egyetlen adatbázis szerepkört, mint az adatok elkülönítési technika viszonteladótól azonosítójú csoportban a rendszer a interaktív módon.

Mivel a vállalati összes információ a későbbi effektusok (További üzleti ösztönzése az kereskedők, arra ösztönzi meghirdetése és előfizetés a létrehozás) azt is rajzolja meg a vonalat a vásárlások kezelése az interneten keresztül. Ilyen nem található a bevásárlókocsi webhelyünkön, amely megkönnyíti a biztonsági követelményeknek. 

Alkalmazott azt egy másik egyszerűsítése a számlázás és -fiókok fizetendő műveletek kihelyező volt. Útválasztási ügyfél fizetési információk közvetlenül egy harmadik fél által ([SveaWebPay](http://www.sveawebpay.se/)), hogy tárolásához és az adatokat tárolja a kényes adatok védelme társítása kockázatok csökkentése. 

### <a name="search-engine"></a>Keresőmotor

Az alapvető a megoldás a keresőprogram-ra épülő Azure keresési szolgáltatás. Kezdetben úgy alakítottuk ki egy egyéni keresőmotor, de a folyamat során azt történjen realizálása összetettségétől és munkamennyiség nagyon magas volt valóban, és kéri, hogy más alternatívák fontolja meg, hogy mi. 

Főbb tulajdonságait, amelyekre legfontosabb nekünk tartalmazza:

- Szűrők
- Síklapos navigációs
- Eredmények szolgáló lehetőségekről
- Lapozás AJAX keresztül

Az alábbi videóban, amely ösztönző nekünk Azure keresési próbálja us hozni egy internetes keresés: [Mély merülési TechEd Europe:](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Azt követően a videolejátszás volt készen áll egy minta alapján azt látta létrehozására. Azt már adatmodell MVC, mert a prototípusának létrehozása volt túl bonyolult feladat, mert az adatok kereshető kifejezéseket tartalmaz, és azt is már megállapítási követelményei, hogy hogyan azt szeretett volna dimenzió, rendezheti és szűrheti az adatokat. 

Ez a hogyan úgy alakítottuk ki a prototípusának.

**Azure keresési szolgáltatásának konfigurálása**

1. Jelentkezzen be Azure klasszikus Portáljára, és a keresési szolgáltatás hozzáadása az előfizetéshez. A megosztott (ingyenes, az előfizetéssel rendelkező) verziót használja azt.
2. Index létrehozása. A prototípusának hogy a felhasználói felület portál meghatározása a kereséshez használható mezők, és a pontozási profilok létrehozása használni. A pontozási profil helyadatok alapul: ország |} a város |} cím (lásd: pontozási profilok hozzáadása).
3. Másolja a szolgáltatás URL-CÍMÉT és a rendszergazda a konfigurációs fájl api-billentyűvel. Ez a szolgáltatás lapon a portálon kulcsa, és segítségével hitelesíteni a szolgáltatásban.
    
**A keresési indexelő feladat – Windows konzol kidolgozása**

1. Olvassa el a minden viszonteladói adatbázisból.
2. Hívja fel az Azure keresési szolgáltatás API viszonteladói egyesével feltöltése (lásd: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Tulajdonság egy adatbázisban, amely az indexelés egyre növekvő tendenciát mutat viszonteladótól indexelt. Azt egy "indexelő" mezőt, amely az egyes profilok (vagy ne indexelt) index állapota tárolja hozzáadásával indult el. 

Lásd a kódtöredék, hogy az indexelő feladat hozza létre.

**A Keresés webes portál – MVC kidolgozása**

1. Hívja fel az Azure keresési szolgáltatás úgy juthat az összes dokumentum keresés (lásd: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. A keresési szolgáltatás válasz követését (json.net http://james.newtonking.com/json) segítségével kivonat
   - Eredmény
   - Metszettel
   - Eredmény száma
   - Fejleszthet olyan találatok, metszettel és száma (azt már ez) a felhasználói felület.

Ez a kód használt Azure keresési eredmény érhető el:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Szolgáló lehetőségekről elhelyezkedés szerint**

Valószínűleg a legfontosabb követelmény, a hely keresés kulcsszó fel a lekérdezésbe felvenni prototípusának ellenőrzéséhez. Fontos a portálra lekérdezésére, ha egy felhasználó a város neve ír be a keresőmezőbe, hogy az adott város a viszonteladói magasabb a város kulcsszót a leírás viszonteladók volna rangsorolása. Ez a követelmény azt pontozási profil rangsorának a Város mező nagyobb, mint a többi mező használni.

**Több nyelv támogatása**

Azt szükséges megfelelő találatok megjelenítése a megfelelő nyelven, és küldje el az egyéb nyelvek ugyanazt az eredményt kereséséhez beállítást. Azok a két oldal erre a problémára: 

- Több nyelv szavak keresése
- A megfelelő nyelvi megjelenítendő találatok

A dokumentumokban a minden nyelv honosított szöveggel és egy tulajdonság, a nyelv megadásával megoldani azt a bemutató kijelzőt. Amikor a felhasználó ír be egy keresőkifejezést azt felhasználói `$filter` kifejezések szűrni a nyelvet a felhasználó által választott.

Az egyes dokumentumok tartalmaz egy rejtett tulajdonság neve "városok" a gyűjtemény típusa épül. Ez a tulajdonság minden nyelv engedélyezése a Keresés a többnyelvű felhasználói város nevét tárolja.

###<a name="data-storage"></a>Adattárolás

Az összes adat (profilját, előfizetés és számlázás) SQL-adatbázisban vannak tárolva. Az összes médiafájl Azure blobtárolóhoz, beleértve a képeket és videókat a viszonteladótól által biztosított tárolja. Külön BLOB-tárolóhoz használatával elkülöníti a fájlok feltöltése; hatásai a fájlok soha nem közös mingled a webhelyet, ezért nincs szükség a webhely újjáépítése, valahányszor hozzáadunk fájlok.

Egy fontos a tárhely tervezés előnye, hogy több fejlesztők megoszthat egyetlen fejlesztési tárolási. A WhatToPedia projekthez a követelmények egyikének volt, létrehozhatnak egy fejlesztői környezet 15 percet, beleértve a viszonteladói adatok, képek és videók belül. A legfrissebb adatok lekérése TFS Online, egy SQL-parancsprogram futtatása és az importálás futtatása, teljes körű környezetet is lehet formája használatba egyáltalán. A gyakorlatban is javítja az átmeneti tárolásra szolgáló folyamat.

###<a name="webjobs"></a>WebJobs

Azure WebJobs segítségével adatok frissítése az indexhez. A keresési indexelő feladatok létrehozásával az indexelési része nagyon egyszerűen a megoldást integrálni szeretné a volt. A csak végeztünk kód módosítása volt, kiterjesztve azt az indexelő feladat volt hozzáadása egy `Indexed` mező az index állapot jelzése az adatmodellbe. Új profil hozzáadásakor vagy frissítésekor, amikor a `Indexed` mező értéke hamis. Ugyanez érvényes, ha a viszonteladótól megváltozik partnerlistájukra profiladatok a portálon keresztül.  

A projekt összes sorát, amelynek formátumban `Indexed` hamis értékre van állítva. A sorban keres, a dokumentum fel a program Azure keresés, majd a `Indexed` mező értéke igaz. Nem kell hozzáadása és az adatok frissítését, mert az Azure keresési szolgáltatás a ténylegesen gondoskodik megtervezése. Ha felvesz egy dokumentumot, amely már szerepel, a szolgáltatás automatikusan frissítés hajt végre.

Projektek webes kidolgozott Azure-webhelyek ZIP-fájlokat feltölteni, kibontott, majd ütemezett konzol alkalmazásként.

A feladat ütemezett webes feladatként 5 percenként van ütemezve. A Microsoft számítja ki, hogy a szolgáltatás körülbelül percet vesz igénybe három feltöltése 3000 dokumentumokat, amelyek a követelmények belül lett. 

> [AZURE.NOTE] Van olyan prototípusának indexelő szolgáltatás, amely a legutóbb Azure keresés jelent meg. Ez a funkció későn nekünk az az első megjelenés programban használandó volt, de a azonos probléma megoldására azt használja az indexelő feladat, amely olyan, automatizálhatja a betöltés adatműveleteket megjelenik.


###<a name="backup-strategy"></a>Biztonsági mentési stratégia

Több többszintű biztonsági stratégia esetben Katasztrofális hiba, le egy adott tranzakció helyreállítási cellatartományban lévő visszaállításához készült azt. Az eszközök védelme három típusú adatokat (a webhely, a előfizetői adatok és a médiafájlok) tartalmazza. 

Első lépésként a webhely-forráskód tartása TFS Online, amelyet tudjuk, hogy ha a webhely megszakad, azt is újraépítéséhez azt a TFS közzétételével. 

Előfizetői Azure SQL-adatbázis adatai a legtöbb bizalmas eszköz. Azt vissza használatával a beépített Ez a funkció (lásd: [Azure SQL-adatbázis biztonsági mentése és visszaállítása](http://msdn.microsoft.com/library/azure/jj650016.aspx)). A biztonsági mentés ütemezése teljes adatbázis biztonsági másolatának hetente egyszer, megkülönböztető adatbázis biztonsági mentése egyszer, de tranzakció napló biztonsági másolatainak 5 percenként.  Az adatok méretének adni, a megoldás, több, mint megfelelő-e a azonnali és a tervezett adaton.

Harmadik kép-és videofájlokat tároljuk Azure BLOB-tárolóhoz. Azt is értékelése ezeket az adatokat ultimate biztonsági mentés megtervezése Cloudberry Explorer, a potenciális megoldásként Azure figyelembe. Azt használni egy WebJob képek és videók másolása egy másik helyre.

##<a name="what-we-learned"></a>Azt a tanultakhoz

Volt már adatok, mert volt létesíthet vásárlási a fogalom, egyszerűen. Órán belül egy prototípusának megnyilvánulása volt számláló, személyhívó, Microsoft Excel úgy rangsorolja profilok és a keresési eredmények. A keresési eredmények volt, így pontos, hogy úgy döntött, hogy távolítsa el a szűrőket, a befejezési ügyfélnek bemutatott részét. 

A legfontosabb jelzés nélküli nekünk volt, hogy milyen gyorsan azt is megtudhatja Azure keresési, és mennyi azt elmehetett. Betűhíven hogy folyamatban vásárlási a fogalom, (lásd a lenti megjegyzést) néhány órát, 500 kódsorokat helyettesít 3 kódsorokat az előtér-alkalmazás (plusz egy új WebJob), és jobb eredmények. 

Korábban a kód végrehajtani, lapozás, száma és más viselkedések, amelyek a normál kereséséhez. Azure keresési használ, az eredményt, akkor visszatérhet a keresési találatok, metszettel, adatok, megszámolja – hogy szükség, és azok kellene adnia ragozott formáival összes elemek személyhívó tartalmazza. Azt is szolgáló lehetőségekről és beépített síklapos navigációs, amely azt az eredeti megoldásban nem volt telepítve.

A legnagyobb beavatkozás igazolására szolgáló eljárás a végrehajtás során előzetes verziójára volt és a megosztott elemek és információk keresése volt túl bonyolult volt. Azt a pontot néhány csatlakozás után talált, hogy meglehetősen egyszerű miatt a REST API-val és a JSON adatformátum Azure keresési szolgáltatás használatával volt. Azt is hívja fel az keretrendszer közvetlenül a legtöbb megnyitott forrás bővítmények például JQuery JSON.Net, és azt is használható eszközök, például Fiddler gyors kísérletezés és hibakeresés. 

> [AZURE.NOTE] Amellett, hogy az adatok prepped problémákat, akkor segíteni azok megfelel a prototípusának már épület érthető keresés technológia működése, kezdeményezhet us hatékonyabban, és további appreciative beépített funkciói. Ha emelkedő be a keresési lekérdezés építés kell síklapos navigációs, szűrők, stb kell várt prototípusának elkészítéséhez a hosszabb időt vehet igénybe. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>A keresés bemutató lapon metszettel szabályozása

Gondosan upfront metszettel megtervezésére vásárlási-a-amely során az learnings egyik volt. Nagy mennyiségű adat betöltése a megoldásba, után bekerül, hogy a metszettel puszta hangerejének túl nagy a felhasználóknak történő volt. 

Hogy korlátozza a dimenzió count paraméter által megoldani el. A count paraméter ró a metszettel visszatér a felhasználó számára nehezen korlátozott. Egy hivatkozás, amely tartalmazza a darab paraméter vitafórum-témakörökben találhatók [Itt](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>Tevékenységeknek az ütemezésében való WebJobs

Azure keresés nem lett a csak pleasant jelzés nélküli nekünk. Azt a feltárással, hogy az előző megközelítés, és használata Windows ütemező rendszerrel ütemezett tevékenységek frissítése a keresési index egy dedikált virtuális járó nagyban kiváló WebJobs használatával automatizálhatja a betöltés adatműveleteket Azure keresés volt. WebJobs volt, egyszerűbb konfigurálása és könnyebben hibakeresési és természetesen lényegében nem kell fizetnie egy dedikált virtuális olcsóbbak.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure BLOB tároló Explorer képek frissítése

Talált, amely azt a webhelyet, kép és a videó frissítések kezelésében nagyon hasznos a [Azure BLOB-tároló Explorer](https://azurestorageexplorer.codeplex.com/) (funkcióhoz a Codeplex webhelyen érhető el) segítségével. Azt annak segítségével a fejlesztői eszközként kézi frissítése a képek és videók fő webhelyünkön részét képező. Azt, hogy legyen a rugalmasabb, mint módosítások bevezetéshez a portálon található, és megszünteti a teljes vizsgálati közelítését, valahányszor kell a kép módosítása. 

##<a name="a-few-final-words"></a>Néhány szövegrészt

Köszönetnyilvánító WhatToPedia a kiváló emberek lehetővé tevő számunkra, hogy a szövegegység megosztása!  

Azt a projektet a esettanulmány hasznos található. Ha megnyitja Azure kereséssel, lehet, mentén gyorsabb néhány erőforrások javasoljuk:

- [Azure keresés kitűzött célja fórum az MSDN webhelyen](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow szintén címke](http://stackoverflow.com/questions/tagged/azure-search)
- [Azure.com dokumentáció lapján](https://azure.microsoft.com/documentation/services/search/)
- [Azure keresési dokumentáció MSDN webhelyen](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>. Melléklet: Keresési indexelő WebJob

A következő kód hozza létre az indexelő a prototípusának épület részben leírt.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
