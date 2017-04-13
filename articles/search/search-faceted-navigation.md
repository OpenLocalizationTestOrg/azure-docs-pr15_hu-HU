<properties 
    pageTitle="Síklapos navigációs megvalósításáról Azure keresés |} Microsoft Azure |} keresési navigáció kategóriák" 
    description="Adja hozzá síklapos navigációs integrálása az Azure keresést, a felhőben tárolt keresési szolgáltatás a Microsoft Azure-alkalmazások." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Síklapos navigációs megvalósításáról Azure-keresés

Síklapos navigációs egy szűrési eljárást, amely a keresési alkalmazások önálló irányított részletezés navigációs biztosít. Lehet, hogy a kifejezéskészlet "síklapos navigációs" ismeretlen, azt is majdnem olyan tekintve, hogy a használt elé. Ahogy az alábbi példában látható, a síklapos navigációs semmi több, mint a kategóriák szűrhető eredménye.

## <a name="what-it-looks-like"></a>Hogy néz ki

 ![][1]
  
Metszettel segítséget mi keres, biztosítva, hogy nem jelenik meg eredmény. Fejlesztői metszettel lehetővé teszik a hasznos Navigálás a keresési corpus keresési feltételek nézetéhez. Online kiskereskedelem alkalmazásokban síklapos navigációs gyakran épül márka, részlegek (Gyerekszoba cipők), méretét, ár, Népszerűségi és minősítések fölé. 

Végrehajtási síklapos navigációs eltér a különböző keresési technológiák és a nagyon összetett lehet. A keresés Azure síklapos navigációs lekérdezés időben beépített a séma korábban megadott kiadott mezők használatával. Az alkalmazás hozza létre a lekérdezésben lekérdezés kell küldenie a *dimenzió lekérdezés paraméterei* kapni a rendelkezésre álló dimenzió szűrőértékek a dokumentumkészlet eredmény érdekében. Ténylegesen levághatja a dokumentum eredmény beállítása, telepítenie kell az alkalmazás egy `$filter` kifejezés.

Alkalmazások fejlesztése, értelmez munka tömeges lekérdezések szerkezetek kódírás képez. Az alkalmazás működésének kívánt volna síklapos navigációs elemek közül számos a szolgáltatást, többek között a tartományok beállításáról, és megszámolja első dimenzió eredmények beépített támogatása által biztosított. A szolgáltatás is tartalmaz, amelyek segítségével kijelölve alapértelmezett kezelhetővé vált navigációs szerkezet elkerülésére. 

Ez a cikk az alábbi szakaszok tartalmazza:

- [Hogyan hozhat létre](#howtobuildit)
- [A bemutató réteg összeállítása](#presentationlayer)
- [Létrehozhatja a tárgymutatót](#buildindex)
- [A adatok hangminőség ellenőrzése](#checkdata)
- [A lekérdezés összeállítása](#buildquery)
- [Hogyan szabályozható síklapos navigációs kapcsolatos tippek](#tips)
- [Síklapos navigációs tartomány értékek alapján](#rangefacets)
- [Síklapos navigációs GeoPoints alapján](#geofacets)
- [Próbálja ki](#tryitout)

##<a name="why-use-it"></a>Miért érdemes használni
A lehető leghatékonyabb kereső alkalmazások több kapcsolati modellek van, a keresőmező mellett. Síklapos navigációs található egy alternatív szeretne keresni, kézzel írja be az összetett keresési kifejezések kényelmes alternatívájaként kínáló.

##<a name="know-the-fundamentals"></a>Tudnivalók a alapjai

Ha új fejlesztés kereséséhez, a legegyszerűbben úgy gondolja, hogy a síklapos navigációs, hogy látható-e saját irányított keresés lehetőségeit. Lehatolás keresési élményt, előre definiált szűrők, gyorsan szűkíteni a keresési eredmények között pontot, és kattintson a műveletek használható egy adott típusú. 

**Kapcsolati modell**

A keresési élményt a síklapos navigációs közelítéses, így érdemes tudnivalók a lekérdezések, amelyek a felhasználói műveletek válaszként unfold sorozata, először.

A kiindulási pont-alkalmazás lap, ahol síklapos navigációs általában kerülete helyezett el. Legtöbbször síklapos navigációs fastruktúrában, a minden érték vagy szöveg kattintható mellől. 

1.  Azure keresési küldött lekérdezés keresztül egy vagy több dimenzió lekérdezés paraméterei síklapos szerkezeti adja meg. Ha például a lekérdezés tartalmazhatnak olyan `facet=Rating`, esetleg egy `:values` vagy `:sort` tovább finomíthatja a bemutató lehetőséget.
2.  A bemutató réteg a Keresés lapot, ahol síklapos navigációs sávja, használja a megadott a kérésre metszettel jeleníti meg.
3.  Minősítés tartalmazó síklapos szerkezeti adni, a felhasználó kattint "4" jelzi, hogy csak a 4-es vagy újabb egy minősítéssel rendelkező termékek jelenjenek meg. 
4.  A válasz az alkalmazás küld egy lekérdezést, amely tartalmaz`$filter=Rating ge 4` 
5.  A bemutató réteg frissíti a lapot, amely az csökkentett az eredményhalmaz, csak az adott tesz eleget az új cikkeket tartalmazó (ebben az esetben a termékek minősítette 4-es és legfeljebb).

Egy dimenzió lekérdezési paraméter, de ne tévessze össze azt a lekérdezés beviteli. Soha nem használják a kijelölés feltételként egy lekérdezésben. Ehelyett érdemes dimenzió lekérdezés paraméterei bemeneti adatok alapján, a navigációs struktúra, hogy az vissza az adott válasz megjelölése. Minden dimenzió lekérdezési paraméter ad az Azure keresési értékeli hány dokumentumok szerepelnek, az egyes dimenzió értéke részleges eredményeket.

Értesítés a `$filter` a lépés: 4. Ez a síklapos navigációs fontos eleme. Habár metszettel és szűrők: a API független, szüksége lesz mindkettőt kitöltési élményt. 

**Tervező mintázatok**

Az alkalmazás kód a mintázat környezetbe dimenzió lekérdezés paraméterei való visszatéréshez síklapos szerkezeti együtt dimenzió eredményeket, valamint egy $filter-kifejezés, amely a click esemény kezeli. A gondolkodási a `$filter` a keresési eredmények a tényleges elrejtést mögött kódként kifejezés a bemutató réteg adja vissza. A színek dimenzió adni, kattintson a vörös színt alkalmazása keresztül egy `$filter` kifejezés, amely csak a vörös színt rendelkező elemeket kijelöli. 

**Azure keresési lekérdezés alapjai**

Azure keresés kérelmének keresztül (lásd: [Dokumentumok keresése](http://msdn.microsoft.com/library/azure/dn798927.aspx) mindegyik leírása) egy vagy több lekérdezési paraméter van megadva. A lekérdezés paraméterei sem kötelező, de kell rendelkeznie ahhoz, hogy érvényes lekérdezés közül legalább egyet.

Pontosság, az azt jelenti, hogy a találatok nem számít, kiszűrése általában érteni egyik vagy mindkét alábbi kifejezések keresztül érhető el:

- **Keresés =**<br/>
Ez a paraméter értéke jelent, a keresési kifejezés. Lehet, hogy a szöveg vagy egy összetett keresési kifejezés, amely tartalmazza a kifejezések és a operátorok egy darab. A kiszolgálón a keresési kifejezés használható teljes szöveges keresési kifejezések – a sorrend ad az eredményt megfelelő a tárgymutató kereshető mezők lekérdezése. Ha a `search` lekérdezés ra a végrehajtás véget ért a teljes indexben (Ez azt jelenti, hogy `search=*`). Ebben az esetben az egyéb elemek a lekérdezés, például egy `$filter` vagy pontozás, a profil lesz az elsődleges tényezőket, mely dokumentumokat érintő visszaadott `($filter`), és milyen sorrendben (`scoringProfile` vagy `$orderb`y).

- **$filter =**<br/>
Egy hatékony mechanizmusa korlátozza a keresési eredmények az adott dokumentum attribútumok értékein alapuló méretének szűrő. A `$filter` van kezdődni, hozza létre a rendelkezésre álló értékeket és a megfelelő száma értékenként faceting logikájának követ

Összetett keresési kifejezés a lekérdezési teljesítmény csökkenni fog. Ha lehetséges, használja a pontosság növelése és a lekérdezési teljesítmény javítása jól épített szűrőkifejezések.

Jobban megértheti hogyan szűrő felveszi a pontosabb, hasonlítsa össze egy összetett keresési kifejezésre, ami szűrőkifejezés tartalmazza:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Habár mindkét lekérdezésekre érvényes, a második pedig a felettes, ha nem motelek a Seattle rögzítő a keresett. Az első lekérdezés listázzák említett vagy a nem, például a név, leírás és bármely más mezőre kereshető adatokat tartalmazó karakterlánc mezőket említett támaszkodik. A második lekérdezés keres pontos egyezést strukturált adatok, és valószínűleg pontosabb sokkal.

Síklapos navigációs tartalmazó alkalmazásokban azt szeretné, hogy meggyőződhessen róla, hogy minden felhasználói művelet fölé síklapos szerkezeti fűzni a keresési eredmények beolvasásához, szűrőkifejezés elérni szűkíteni.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Hogyan hozhat létre

Azure keresés síklapos navigációs alkalmazása a alkalmazás kódban, hozza létre a kérést, de a sémában előre megadott elemek támaszkodik.

Előre definiált a keresési index van a `Facetable [true|false]` index attribútumának, a kijelölt mezők engedélyezéséhez vagy letiltásához síklapos navigációs struktúrában is használatuk. Nélkül `"Facetable" = true`, egy mező dimenzió navigációs nem használhatók.

Lekérdezés időben, az alkalmazás kódja összehívást hoz létre, amely tartalmazza `facet=[string]`, ahol a mező által dimenzió kérelem paraméter. A lekérdezés beállíthatja, hogy több metszettel, például: `&facet=color&facet=category&facet=rating`, egymástól úgy, hogy az ampersand (&) karaktert.

Alkalmazás kódja is kell létrehozniuk egy `$filter` síklapos navigációs sávján kattintson a események kezelése kifejezés. A `$filter` csökkenti a keresési eredmények beolvasásához, használja a dimenzió értéke szerint szűrőfeltételeket.

Azure keresés megjeleníti a keresési eredmények per a frissítések síklapos szerkezeti együtt, a felhasználó által megadott kifejezést. Azure keresés síklapos navigációs egy egyszintű építés dimenzió értékű, és megszámolja, hogy hány eredmények találnak mindegyiknél.

A bemutató réteg a kódot a felhasználói felületet nyújt. A síklapos navigációs elemek, például a címkét, érték, jelölőnégyzetek és darabszámát elemeire szerepelnie kell azt. Az Azure keresési REST API platform felismerése nélkül, így a bármilyen nyelvet és a platform, amelyet használni. A fontos megoldás, ha olyan felhasználói felület elemeket, amelyek támogatják a növekményes frissítés, a frissített felhasználói felület állapot, minden további dimenzió ki van jelölve. 

Az alábbi szakaszok fogja vesszük hogyan hozhat létre az egyes részek, részletesebben a bemutató réteg kezdve.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>A bemutató réteg összeállítása

Vissza a bemutató réteghez használata könnyíti meg a Kihúzás milyen lehetőségei lényeges, hogy a keresési élményt, egyéb esetben kihagyott, és megértette követelmények.

Értelmez síklapos navigációs sávján a web-, illetve lap síklapos szerkezeti jeleníti meg, észleli a felhasználó által a lapon és szúrja be a módosított elemhez. 

A webalkalmazások esetében AJAX általában arra használják a bemutató rétegben mivel lehetővé teszi, hogy növekményes változtatások frissítése. ASP.NET MVC vagy bármely más képi megjelenítés platform, amely a HTTP protokollon keresztül csatlakozhat az Azure keresési szolgáltatás is használhatja. Az egész – **AdventureWorks katalógus** – Ez a cikk hivatkozott minta alkalmazás ASP.NET MVC alkalmazás kell történik.

A következő példában a minta alkalmazásának **index.cshtml** fájlból írt egy dinamikus HTML-struktúrájának megjelenítését befolyásoló síklapos navigációs a keresési eredményeket tartalmazó lap hoz létre. A minta síklapos navigációs a keresési eredmények lapja beépített, és a felhasználó elküldte a kívánt keresőkifejezést követően jelenik meg.

Értesítés, hogy minden dimenzió oszlopfejlécek (színek, a kategóriák árak), egy kötelező síklapos mezőhöz (színét, kategórianév, listPrice), és egy `.count` paraméter használt dimenzió eredményhez tartozó található elemek számát adja vissza.

  ![][2]
 

> [AZURE.TIP] A keresési eredmények lapja tervezésekor ne feledje, hogy a kisalkalmazások metszettel törlésével hozzáadása. Ha jelölőnégyzeteket, felhasználók könnyen is intuit hogyan kell a szűrő törlése. Más elrendezésekhez szükség lehet a webhely-navigációs mintát vagy egy másik kreatív megközelítés. Például az AdventureWorks katalógus minta alkalmazásban rákattintva AdventureWorks katalógushoz, a Keresés lap visszaállítása a címet.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Létrehozhatja a tárgymutatót

A tárgymutató keresztül az index attribútum alapon mező szerint engedélyezve van a faceting: `"Facetable": true`.  
Az összes típusú mező, amely esetleg felhasználható síklapos navigációs vannak `Facetable` alapértelmezés szerint. Az ilyen típusú mező tartalmazza `Edm.String`, `Edm.DateTimeOffset`, és a numerikus mező diagramtípusokat (lényegében összes mezőtípusok-e facetable kivételével `Edm.GeographyPoint`, síklapos navigációs nem használható). 

Index készítésekor síklapos navigációs a legjobb módszer is kifejezetten kikapcsolása faceting a mezőket, amelyeket egy dimenzió soha nem kell használni.  Mindenekelőtt egyszeres az értékek, azonosító vagy a termék neve, például a karakterlánc-mezők értékre kell állítani `"Facetable": false` , hogy egy síklapos navigációs véletlen (és nem hatékony) használatukat.

Az alábbiakban a séma az AdventureWorks katalógus minta alkalmazáshoz (teljes méretének csökkentése érdekében néhány attribútumok díszítve):

 ![][3]
 
Megjegyzendő, hogy `Facetable` metszettel, például azonosító vagy a név ne használható karakterlánc mezőkhöz ki van kapcsolva. Kapcsolja ki, hogy nem kell faceting segít a méretének csökkentése érdekében az index kisebb, és általában javítja a teljesítményt.

> [AZURE.TIP] A legjobb módszer a az egyes mezők indexet attribútumok teljes készlete tartalmazza. Bár `Facetable` alapértelmezés szerint be szinte minden mezők szándékosan beállítása minden attribútum segítséget nyújtanak a gondolja, hogy az egyes séma döntés a következmények keresztül. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>A adatok hangminőség ellenőrzése 

Bármely adatközpontú alkalmazás fejlesztésekor az adatok előkészítése az gyakran egyik a projekt nagyobb részeinek. Keresés alkalmazások nem kivétel. Az adatok minőségének e síklapos szerkezeti megvalósul, ahogyan azt gondolná, adni, és segítségével csökkentheti az eredmény szűrőket Egyenletszerkesztővel hatékonyságának közvetlen hatással van.

Azure keresés a keresési corpus index feltöltéséhez dokumentumokból lett létrehozva. Dokumentumok metszettel kiszámításához használt értékeket adja meg. Dimenzió arculatának vagy ár szeretné, ha minden dokumentum értékeket kell tartalmaznia *BrandName* és *ProductPrice* , amelyek érvényes, egységes és hatékonyabban, mint egy lehetőséget.

Mi a csúszkákkal az néhány emlékeztetők alatt jelennek meg:

- Minden mező által dimenzió kívánt kérdés hogy tartalmaz-e saját irányított keresés szűrőként megfelelő értékeket. Az értékek rövid leíró és eléggé jellegzetes kínálatát törlése választhatnak versengő beállításai között kell lennie.
- Elütések okozta vagy majdnem egyező értékeket. Ha a színt, és a mezőértékek dimenzió narancssárga és felvenni Ornage (helyesírási), a Szín mező alapján dimenzió volna elhozatala mindkét.
- Vegyes eset szöveget is lehet teszi pusztítást síklapos navigációs sávján a narancssárga és a narancssárga két értékként jelennek meg. 
- Az azonos értékkel egyetlen és a többes számú verziók minden egyes eredményezhet egy külön dimenzió.

Ahogy azt el is lehet képzelni, az adatok előkészítése gondosságnál a hatékony síklapos navigációs lényeges eleme.

<a name="buildquery"></a>
##<a name="build-the-query"></a>A lekérdezés összeállítása

A lekérdezések létrehozásának kézzel írt kódot meg kell határoznia, hogy érvényes lekérdezés, például a keresési kifejezések, metszettel, szűrők, pontozási profilok – bármi kérelmének megfogalmazásához használt összes részei. Ebben a részben mutatunk be a hol metszettel illeszkedik-e a lekérdezés és a szűrők használata megnyilvánulása csökkentett eredményhalmazt ad.

Példa egy jó helyen jár, ha gyakran. A következő példában a **CatalogSearch.cs** fájlból írt hoz létre, amely alapján a színt, a kategóriát és az árat dimenzió navigációs létrehoz egy kérelmet. 

Figyelje meg, hogy a metszettel szerves az minta alkalmazásban is. A keresési élményt AdventureWorks katalógus síklapos navigációs és a szűrők körül lett tervezve. Ez a egyértelmű hangjelzéseket helyzetének síklapos navigációs lapon található. A minta alkalmazás URI paramétereinek metszettel (szín, kategória, árak) (mint kialakítani a minta alkalmazásban) keresési módszer tulajdonságként is tartalmaz.

  ![][4]
 
Dimenzió lekérdezési paraméter értéke egy mezőt, és attól függően, írja be az adatokat, is lehet további paraméteres vesszővel tagolt lista tartalmazza az `count:<integer>`, `sort:<>`, `intervals:<integer>`, és `values:<list>`. Értékek listáját támogatott numerikus adatokat tartományok beállításakor. Lásd: [Dokumentumok keresése (Azure keresési API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) használatát részleteket.

Metszettel, valamint a kérést, az alkalmazás által megfogalmazott kell összeállítása a dimenzió értéke kijelölés alapján jelölt dokumentumok készlete szűkítésére szűrők is. Egy kerékpárhoz tároló síklapos navigációs jelek kérdésekre, mint "milyen színek, a gyártók és a Bikes tag típusú érhetők el", szűrés kérdések, például "mely pontos kerékpárok piros, MTB kerékpárok, ez az ár tartomány" válaszokat közben.

Jelzi, hogy csak a piros termékek jelenjenek meg a "piros" felhasználó kattintáskor a következő lekérdezés a kérelem küld tartalmaz `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Gyakorlati tanácsok a síklapos navigációs

Az alábbiakban összefoglaljuk néhány gyakorlati tanácsokat.

- **Pontosság**<br/>
Szűrők használata. Ha a használja arra, csak keresési kifejezések önmagában, amelynek okozhat a pontos dimenzió értéke, amely nem tartalmaz a mezőkre eredményül adott dokumentum. 

- **Célmezők**<br/>
Síklapos Lehatolás általában szeretne csak olyan dokumentumok, amelyek a dimenzió értéke egy adott (síklapos) mező tetszőleges nem minden kereshető mezőkben. A célmezőt szűrő hozzáadása megerősíti úgy irányítja a szolgáltatást a keresés csak a egyező értéket síklapos jelölőnégyzetét.

- **Index hatékonyság**<br/>
Ha az alkalmazás síklapos navigációs kizárólag (Ez azt jelenti, hogy nincs keresőmezőbe), a mezőt jelölheti ki `searchable=false`, `facetable=true` kapcsol tömörebb index létrehozása. Ezeken kívül az indexelés fordul elő a teljes dimenzió értékeket, a word-sortörés nélkül vagy Többszavas érték összetevő részeinek indexelés csak.

- **Teljesítmény**<br/>
Szűrők jelölt dokumentumok keresés kitűzött szűkítéséhez és kizárása őket a rangsorolást. Vannak olyan egy nagyméretű dokumentumokat, ha a egy nagyon szelektív dimenzió részletező lefelé gyakran felkínálja jelentősen jobb teljesítményt.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Hogyan szabályozható síklapos navigációs kapcsolatos tippek

Az alábbi van egy tipp lapot az adott problémák útmutatást.

**Írja be az egyes mezők címkéket dimenzió navigációs sávján**

Címkék általában a HTML- vagy űrlaptár (**index.cshtml** a minta alkalmazásban) definiálva. Nem nincs API dimenzió navigációs címkék keresése Azure vagy bármilyen másfajta metaadatok.

**Dimenzió mezőket is alkalmazható meghatározása**

Az index a séma határozza meg, hogy mely mezők érhetők el a dimenzió fogja használni a visszahívás. Feltételezve, hogy egy mező akkor facetable, a lekérdezés megadása által dimenzió mezők A mező, amellyel faceting felirat alatt látható értékeket tartalmazza. 

Az értékek minden egyes felirat alatt megjelenő az indexből beolvasható. Például a dimenzió mező értéke a *színt*, a rendelkezésre álló további szűrés értékek az értékek (piros, fekete és így tovább) mező lesz.

Numerikus és dátum-idő érték esetében csak explicit módon beállíthatja a dimenzió mező (például `facet=Rating,values:1|2|3|4|5`). Értékek listáját a következő típusú egyszerűsítése érdekében pontosan megadhatja a Kihúzás dimenzió eredmények be összefüggő tartományt (a bármelyik tartomány numerikus értékek vagy időszakokra alapján) engedélyezett. 

**Dimenzió eredmények vágása**

Dimenzió eredménye a találatok között, amelyek megegyeznek a dimenzió kifejezés található dokumentumok. A következő példában az a *felhő számítások*, a keresési eredmények között 254 elemeket is *belső specifikációja* tartalomtípusként. Elemek, amelyek nem feltétlenül egymást kölcsönösen kizáróak. Ha egy elem feltételeknek mindkét szűrők, egyes számít. Ez akkor lehetséges, ha a faceting `Collection(Edm.String)` mezők gyakran használt dokumentumok címkézésével végrehajtásához.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Általánosságban elmondható hogy dimenzió eredmények folyamatosan túl nagy, akkor javasoljuk, hogy megtalálta, adja meg további szűrők, korábbi szakaszban adjon az alkalmazás felhasználóinak szűkíteni a keresést további beállításainak magyarázata.

**A dimenzió navigációs elemek számának korlátozása**

A navigációs fában síklapos mezőinek van alapértelmezett legfeljebb 10 értéket. Ez az alapértelmezett ide kerülő navigációs szerkezet, mert azt az értéklista kezelhető méretű továbbra is. Az alapértelmezett értéket tartalmazó cellákat számlálnia hozzárendelésével felülbírálhatja.

- `&facet=city,count:5`Itt adhatja meg, hogy csak a rangsorban eredmények tetején található első 5 városok visszaadott dimenzió eredményként. Adott 32 találat, és a "airport" kifejezést, ha megadja a lekérdezés `&facet=city,count:5`, csak az első öt egyedi városok a legtöbbet a keresési eredmények között lévő dokumentumok szerepelnek a dimenzió között.

Értesítés dimenzió eredmények és a keresési eredmények közötti különbség. Keresés eredménye a dokumentumokat, amelyek megegyeznek a lekérdezést. Dimenzió eredménye az egyes dimenzió értéke a találatokat. A példában a keresési eredmények tartalmazni fogja város nevek, amelyek nem a dimenzió besorolás listában (5 ebben a példában). A felhasználó számára láthatóvá válik, akkor hatékonyabbnak metszettel törli, vagy más metszettel város mellett dönt síklapos navigációs keresztül kiszűri eredményeket. 

> [AZURE.NOTE] Tárgyal `count` , ha nincs több kijelzőtípust is lehet áttekinthetőbb legyen. Az alábbi táblázat olyan hogyan a kifejezés az Azure keresési API, példakódot és dokumentáció rövid összefoglalása. 

- `@colorFacet.count`<br/>
Bemutató kódban látnia kell a darab paramétert a dimenzió dimenzió találatok száma megjelenítésére szolgál. Dimenzió között darab azt, hogy a dokumentumok, amelyek megegyeznek a dimenzió kifejezés vagy a tartományt.

- `&facet=City,count:12`<br/>
Dimenzió lekérdezésben beállíthatja, hogy darab értékre.  Az alapértelmezett érték 10, de azt is megadhatja ilyenkor nagyobb vagy kisebb. Beállítás `count:12` 12 felső megfelelő dimenzió között dokumentum darabszám szerint jeleníthető meg.

- "`@odata.count`"<br/>
Válaszban Ez az érték azt jelzi, a keresési eredmények között egyező elemek számát. Átlagosan nagyobb, mint az összes dimenzió eredmény együtt, az elemek, amelyek megegyeznek a keresőkifejezést jelenléte miatt összegét, de nincs dimenzió értéke találat van.


**Síklapos navigációs szintek** 

Amint nem nincs közvetlen beágyazása a hierarchia metszettel támogatása. Beépített síklapos navigációs csak egy szinttel a szűrők használatát támogatja. Jó helyen jár a megoldások létezik. Akkor is kódolását dimenzió hierarchikus struktúrák egy `Collection(Edm.String)` egy bejegyzésnek mutasson egy hierarchiát. A megoldás a jelen cikk túlra, de OData- [példa szerint](http://msdn.microsoft.com/library/ff478141.aspx)gyűjtemények olvashat. 

**Mezők ellenőrzése**

Ha a felhasználó nem megbízható bemeneti dinamikusan alapján metszettel listáját, vagy ellenőriznie kell, hogy a síklapos mezők azoknak a érvényes, vagy a nevek escape-URL-ek a következők egyikét használja készítésekor `Uri.EscapeDataString()` .NET vagy az annak megfelelő a platform megválasztott.

**A függvény összeszámolja dimenzió eredménye**

Amikor szűrő síklapos lekérdezéshez adása, előfordulhat, hogy szeretné megőrizni a dimenzió utasítás (például `facet=Rating&$filter=Rating ge 4`). Értelemben, dimenzió = minősítés nem szükséges, de ezt megtartásáról minősítések dimenzió értékek számát adja eredményül, 4-es vagy újabb verziójában. Például ha a felhasználó "4" kattint, és a lekérdezés nagyobb vagy egyenlő "4" szűrője, megszámolja a visszaadott minden besorolása, 4, és be.  

**Dimenzió megszámolja a sharding következmények**

Bizonyos körülmények között előfordulhat, hogy dimenzió száma nem egyezik meg az eredményhalmazok (lásd az [Azure-keresés (fórum post) síklapos navigációs](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Lehet, hogy a sharding architektúra miatt téves dimenzió száma. Minden keresési index több shards van, és egyenként-jelentések a legnépszerűbb vagy legnagyobb N metszettel eredményének kombinálni dokumentum darabszám szerint. Ha bizonyos shards egyező értékek, míg mások rendelkeznek kisebb sok van, akkor azt tapasztalhatja, hogy az egyes dimenzió értékeket hiányoznak, vagy a számolt a találatok között.

Bár ez a jelenség változhat bármikor, ha ez a jelenség ma, dolgozhat foglalhatja keretbe mesterségesen fúvódnia számának:<number> meg lehessen őrizni a teljes jelentéskészítés az egyes shard nagyon nagy számra. Ha a darab előnyeiről: nagyobb, mint vagy egyenlő mezőjében egyedi értékek számát, akkor garantált pontos eredményeket. Azonban ha dokumentum száma nagyon magas, a rendszer teljesítményét van, így használja ezt a lehetőséget egy.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Egy tartomány értékein alapuló dimenzió navigációs

Tartományok fölé faceting közös keresési alkalmazás követelmény. Tartományok numerikus adatokat, és a dátum-és időértékek támogatott. Erről további tudnivalók a [Dokumentumok keresése (Azure keresési API)](http://msdn.microsoft.com/library/azure/dn798927.aspx)az egyes megközelítés.

Azure keresési tartomány építés egyszerűsíti a azzal, hogy a két megközelítés tartomány kiszámításához. Mindkét megközelítésnek Azure keresési hoz létre a megfelelő tartományokat megadott útjával a bemeneti adatok alapján. Például ha adja meg a 10-es tartomány értékek |} 20 |} 30., akkor automatikusan hoz létre 0 -10, 10 – 20, 20 – 30 cellatartományok. A minta alkalmazás bármely üres intervallumok eltávolítja. 

**1 megközelítés: Intervallum paraméter használatával**<br/>
Metszettel ár 10 USD lépésekben beállításához módon adhatja meg:`&facet=price,interval:10`


**2 megközelítés: Értékek listával**<br/>
Numerikus adatokat, az értékek listáját is használhatja.  Fontolja meg a dimenzió esik listPrice, való megjelenítését a következőképpen:

  ![][5]

A tartomány van megadva, az értékek listákkal **CatalogSearch.cs** fájl:

    facet=listPrice,values:10|25|100|500|1000|2500

Minden olyan tartomány 0 használja kiindulási pontként zárólap, mint a listából egy értéket, és az előző tartomány különálló intervallumok létrehozása majd díszítve beépített. Azure keresési fog ehhez síklapos navigációs részeként. Nem kell az egyes intervallum tagolásának kódírás.

### <a name="build-a-filter-for-facet-ranges"></a>Dimenzió tartományokra vonatkozó szűrő létrehozása ###

A felhasználó által választott tartományon alapuló dokumentumok szűréséhez használni a `"ge"` és `"lt"` szűrése egy két részből álló kifejezés, amely definiálja, a végpontok tartomány operátorok. Ha például a felhasználó úgy dönt, a tartomány 10-25, a szűrő akkor `$filter=listPrice ge 10 and listPrice lt 25`.

A minta alkalmazásban a filter kifejezésére **priceFrom** és **priceTo** paramétereket használ a végpontokat. A **Szűrő létrehozása** **CatalogSearch.cs** módszer, amellyel a dokumentumokat egy tartományon belül szűrőkifejezés tartalmazza.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Szűrt navigációs GeoPoints alapján

Annak közös megtekintéséhez szűrők, amelyek segítenek úgy dönt, hogy egy áruházból, éttermi vagy annak közelében aktuális tartózkodási alapján cél. Az ilyen típusú szűrőt síklapos navigációs nézhet, azt is valójában csak egy szűrőt. Itt azokat meg, hogy az adott tervezési probléma tanácsok végrehajtása kifejezetten keres azt említeni.

Rendszer Azure keresési, **geo.distance** és **geo.intersects**két térinformatikai függvényt.

- A **geo.distance** függvény a távolságot a kilométer két pontok közötti, egy mező és állandó egyik átadott a szűrő részeként. 

- A **geo.intersects** függvény értéke igaz, ha egy adott pont belül egy adott sokszög, ahol a pont mező és a szűrő részeként átadott koordináták állandó listáját a sokszög van megadva.

Szűrő példákat találhat [OData kifejezések szintaxisával (Azure keresés)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Próbálja ki

Azure keresési Adventure Works bemutató funkcióhoz a Codeplex webhelyen a hivatkozott Ez a cikk példákat tartalmaz. Munka közben a keresési eredmények között, nézze meg a lekérdezés építés változásai URL-CÍMÉT. Ez az alkalmazás metszettel hozzáfűzése a URI mindegyik kijelölésekor történik.

1.  Konfigurálja a minta alkalmazást használja a szolgáltatás URL-CÍMÉT és az api-billentyűt. 

    Figyelje meg, amely a CatalogIndexer projekt Program.cs fájlban definiált a séma. Azt adja meg a színt, listPrice, méretét, betűvastagságát, kategórianév és modelName facetable mezőket.  Csupán néhány példa, ezek (szín listPrice, kategórianév) ténylegesen végrehajtását síklapos navigációs sávján.

3.  Futtassa az alkalmazást. 

    Az első akkor látható, a keresőmezőbe. Kattintson a Keresés gombra kattintva azonnal az összes eredményt ad, vagy írjon be egy keresőkifejezést.

    ![][7]
 
4.  Írjon be egy keresőkifejezést, például kerékpárhoz, és kattintson a **Keresés**gombra. A lekérdezés gyorsan hajt végre.
 
    Síklapos szerkezeti is a keresési eredményekkel adja vissza.  Az URL-címet színek, a kategóriák és árak metszettel is null. A keresési eredmény lap síklapos szerkezeti száma az egyes dimenzió eredménye tartalmaz.

     ![][8]
 
5.  Kattintson a szín, a kategória és a ár tartományban. Metszettel null, egy kezdeti Search, de veszik értékek, mint az elemek, amelyek nem felelnek meg a keresési eredmények lesz. Figyelje meg, hogy a URI felveszi a módosításokat a lekérdezést.

    ![][9]
 
6.  A síklapos lekérdezést úgy, hogy a különböző lekérdezés viselkedése megpróbálhatja törléséhez kattintson **AdventureWorks katalógushoz** elemre a lap tetején.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Következő lépés

A Tudásbázis is adhat hozzá egy *modelName*dimenzió mező. Az index már be van állítva az e dimenzió, így szükség-e az index nincs változtatás. De a HTML-kód egy új dimenzió tartalmazzák a modellek, és a dimenzió mező felvétele a lekérdezés konstruktor módosítani kell.

További az összefüggéseket a síklapos navigációs alapelvei tervezés, az azt javasoljuk az alábbi hivatkozások:

- [A keresés síklapos tervezése](http://www.uie.com/articles/faceted_search/)
- [Tervező mintázatok: Síklapos navigáció](http://alistapart.com/article/design-patterns-faceted-navigation)

[Azure-mély merülési keresési](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410)is megtekinthet. A 45:25 nincs egy bemutató metszettel megvalósításáról.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 