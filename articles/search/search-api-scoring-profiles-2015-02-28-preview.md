<properties
    pageTitle="A pontozási profilok (Azure keresési REST API-verzió 2015-02-28 – előzetes verzió) |} Microsoft Azure |} Azure keresési előnézete API"
    description="Azure keresési, amely támogatja a pontozási profilok felhasználói alapján rangsorolt találatok javítása a felhőben tárolt keresési szolgáltatásának."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>A pontozási profilok (Azure keresési REST API-verzió 2015-02-28 – előzetes verzió)

> [AZURE.NOTE] Ez a cikk ismerteti az [2015-02-28 – előzetes verzió](search-api-2015-02-28-preview.md)pontozási profilok. Jelenleg nincs közötti különbség a `2015-02-28` [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) dokumentált verzió és a `2015-02-28-Preview` verzió itt leírt, de mégis kínálunk dokumentumtartalom annak érdekében, hogy a dokumentum kitöltésének révén a teljes API-t.

## <a name="overview"></a>– Áttekintés

A számítási egy keresési pontszámhoz minden elemhez, a visszaadott találatok pontozási hivatkozik. Az érték egy elem relevancia keretén belül az aktuális keresési művelet mutatója. Minél nagyobb pontszámhoz, a fontosabb az elemet. A keresési eredmények között a cikkeket magasabbtól rendelt gyenge; alapján számítja ki az egyes elemekkel keresési eredmény rangját.

Azure keresési használja az alapértelmezett pontozási, egy kezdeti érték kiszámítása, de testre szabhatja a számításban pontozási profil keresztül. Pontozási profilok ad jobban kézben tarthatók az elemek besorolását a keresési eredmények között. Érdemes lehet például növeli a megfelelő elemeket a bevétel lehetséges, újabb elemek előléptetése vagy talán növeli az elemeket, amelyeket közvetlenül a készletben túl hosszú.

A pontozási profilok mezők, a függvény és a paraméterek álló index meghatározása része.

Adjon meg arról, hogyan pontozási profil fog kinézni, a következő példa bemutatja a "geo" nevű simple profil. Ez egy növekedhet elemek, amelyeknek a keresőkifejezést a `hotelName` mezőben. Is használja a `distance` függvény belül az aktuális helyre a tíz kilométer-elemek alkalmazást. Ha valaki a "inn" kifejezést a keresés, és a szállások neve részének történik "inn", "inn" Szállodák tartalmazó dokumentumok nagyobb a keresési eredmények között fog megjelenni.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

A lekérdezés pontozási profil használatához elkészíteni van, hogy a profil adja meg a lekérdezési karakterlánc. Az alábbi lekérdezés figyelje meg a lekérdezési paraméter `scoringProfile=geo` a kérelem.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

A lekérdezés "inn" kifejezést keresi, és a jelenlegi helyen továbbítja. Megjegyzés: Ez a lekérdezés tartalmaz a további paramétereket, például: `scoringParameter`. A lekérdezés paraméterei megkötésekről [Dokumentumok keresése (Azure keresési API)](search-api-2015-02-28-preview.md#SearchDocs).

[Példa](#example) , tekintse át a pontozási profil részletesebb példa gombra.

## <a name="what-is-default-scoring"></a>Mi az alapértelmezett pontozási?

A keresés pontszámhoz sorszám rendezett eredményhalmazok az egyes tételeihez pontozási számítja ki. Keresés az eredményhalmaz minden elemet a keresési eredmény rendelt, majd a legalacsonyabb legnagyobb rangsorban. A magasabb értékek elemek szerepeljenek eredményként az alkalmazás. Alapértelmezés szerint a felső 50 ad vissza, de használhatja a `$top` paraméter való visszatéréshez egy kisebb vagy nagyobb számot (legfeljebb 1000 egyetlen válasz) elemet.

Alapértelmezés szerint a keresési eredmény számított statisztikai tulajdonságait a az adatok és a lekérdezés alapján. Azure keresési talál a keresési kifejezés a lekérdezési karakterláncban tartalmazó dokumentumok (függően néhány vagy összes, `searchMode`), a következő több példányát a keresett kifejezést tartalmazó dokumentumok. A keresési eredmény mutat még nagyobb, ha a szerződési időszak ritka végig az adatok corpus, de a dokumentumon belül közös. A számítási relevancia megközelítése alapját TF-IDF nevezik vagy (kifejezés gyakoriság-inverzének értékét számítja ki a dokumentum gyakoriság).

Feltételezve, hogy nincs egyéni rendezést, a találatok majd rangsorolásának keresési eredmény szerint előtt a hívó alkalmazás kerülnek vissza. Ha `$top` nincs megadva, 50 cikkeknek a legmagasabb keresést, a visszaadott eredmény.

Keresési eredmény értékek teljes eredményhalmaz megismételhető. Ha például egy pontszám 10 cikkeket 1.2-es, lehet, 1.0 pontszám 20 cikket, és a 0,5 pontszám 20 cikkeket. Ha több találat van azonos keresési eredmény, ugyanazokat az elemeket pontszáma megváltoztatva nincs megadva, és nem stabil. Futtatás újra a lekérdezést, és előfordulhat, hogy látható elemek shift helyét. Két elem egy azonos pontszám az adott, nem nincs garancia melyiket először jelenik meg.

## <a name="when-to-use-custom-scoring"></a>Mikor érdemes használni az egyéni pontozás

Egy vagy több pontozási profilokat kell létrehozni, amikor az alapértelmezett működés rangsorolási nem távol elég az értekezlet-az üzleti célok. Dönthet például, hogy a keresési relevancia alkalmazást kell újonnan hozzáadott elemeket. Hasonlóképpen esetleg egy mezőt, amely tartalmazza a profit összege, vagy a potenciális bevétel jelző néhány más mező. Találatok, melyek segítenek előnyeinek üzleti szolgáló lehetőségekről lehet, hogy a fontos tényező úgy dönt, hogy pontozási profilok használata.

Rendezés alkalmazhatóságát-alapú profilok pontozási keresztül is tartalmazzák. Fontolja meg a keresési eredményeket tartalmazó lap, amely lehetővé teszi, akkor az ár, a dátum, a minősítési vagy a relevancia szerinti rendezéshez múltbeli használta. A keresés Azure pontozási profilok meghajtó a "Relevancia" lehetőséget. A definíció megtalálják a számukra lényeges vezérli, való határozza meg a üzleti célkitűzések és a keresési élményt előadása kívánt típusát.

<a name="example"></a>
## <a name="example"></a>Példa

Testre szabott pontozási, amint alkalmazása egy index sémában definiált profilok pontozási keresztül.

Ebben a példában az index és a két pontozási profilok sémájának (`boostGenre`, `newAndHighlyRated`). Ezt az indexet, amely tartalmazza bármelyik profil, paraméteres lekérdezést kell használni a profil pontszám az eredményhalmaz, minden olyan lekérdezése.

[Ez a példa próbálja meg](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Munkafolyamat

A séma, amely definiálja az index pontozási profil hozzáadása egyéni pontozási viselkedés végrehajtásához. Index belül több pontozási profilok is van, de a lekérdezés adott időben csak megadhat egy profilt.

Indítsa el a jelen témakör megadott [sablon](#bkmk_template) .

Adjon egy nevet. Pontozási profilok nem kötelező, de ha beilleszt egyet, ha meg kell adni a nevét. Ügyeljen arra, hogy a mezők elnevezési szabályai kövesse az (egy betűvel kezdődik elkerülhetők a speciális karakterek és fenntartott szavak). [Elnevezési szabályai](http://msdn.microsoft.com/library/azure/dn857353.aspx) talál további információt.

A pontozási profil törzsében összeállítás súlyozott mezők és funkciók.

### <a name="weights"></a>Súlyok ###

A `weights` pontozási profil tulajdonságának neve – érték párokká, egy relatív súly hozzárendelése egy mezőt, amely megadja. A [Példa](#example)a albumTitle műfaj és artistName mezők már csillapítja 1,5, 5 és 2, a kurzor. Miért van így sokkal nagyobb, mint a többi műfaj csillapítja? Ha a keresési mutasson az adatokra, amely kissé homogén végzik (akárcsak a "műfaj" a a `musicstoreindex`), szükség lehet a relatív súlyok egy nagyobb varianciája. Ha például az a `musicstoreindex`, "rock" jelenik meg, mind a műfaj és egyformán phrased műfaj leírások. Műfaj leírás lényegesebbnek műfaj tetszés szerint a műfaj mező az egy sokkal nagyobb relatív súly lesz szüksége.

### <a name="functions"></a>Függvények ###

Függvények használata bizonyos környezetekben működéséhez szükséges további számításai esetén. Érvényes függvény típusok `freshness`, `magnitude`, `distance` és `tag`. Minden egyes függvénynek egyedi rá paramétert.

  - `freshness`Ha azt szeretné, hogy növelése használható hogyan új és régi elem szerint. Ez a funkció csak akkor használható datetime mezőket tartalmazó (`Edm.DataTimeOffset`). Megjegyzés: a `boostingDuration` attribútum csak a frissessége függvénnyel használatos.
  - `magnitude`kell használni, amikor a kívánt növelése alapján hogyan magas vagy alacsony numerikus értéket. Ezt a funkciót a hívó esetek profit összege, a legmagasabb ár, a legalacsonyabb árakat vagy a letöltések számának szolgáló lehetőségekről. A tartomány, a magas, alacsony, hogy akkor visszavonhatja, ha azt szeretné, hogy a inverz minta (például, hogy több, mint magasabb áron elemek alsó árú erősítése cikkek). Egy 100 Ft $ 1 árakat adattartomány tekintve állíthat `boostingRangeStart` 100 és `boostingRangeEnd` : az alsó árú elemek növelése 1. Ez a funkció csak akkor használható, dupla és az egész számot tartalmazó mezővel.
  - `distance`Ha meg szeretné közelség vagy földrajzi hely növelése használandó. Ez a funkció csak akkor használható `Edm.GeographyPoint` mezőket.
  - `tag`Ha meg szeretné növelése közös dokumentumok és a keresési lekérdezések között címkék által használandó. Ez a funkció csak akkor használható `Edm.String` és `Collection(Edm.String)` mezőket.
  
#### <a name="rules-for-using-functions"></a>Szabályok használata a függvények ####

  - Függvény típusú (frissessége, nagyságát, távolságot címke) kisbetű kell lennie.
  - Funkciók nem lehet üres vagy üres értékeket. Kifejezetten Ha mezőnév, akkor állítsa be egy üzenetet.
  - Funkciók csak szűrhető mezők vonatkozni. [Create Index](search-api-2015-02-28-preview.md#CreateIndex) talál további információt a szűrhető mezők.
  - Funkciók csak mezők indexet mezők gyűjteményében definiált vonatkozni.

Miután az index van megadva, létrehozhatja a tárgymutatót által a tárgymutató séma, követi a dokumentumok feltöltése. [Create Index](search-api-2015-02-28-preview.md#CreateIndex) és a [Hozzáadás vagy a frissítés dokumentumok](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) ezeket a műveleteket útmutatást talál. Ha elkészült az indexet, rendelkeznie kell a keresési adatokkal működő funkcionális pontozási profil.

<a name="bkmk_template"></a>
## <a name="template"></a>Sablon
Ebben a részben szintaxisának és profilok pontozási sablonját. Olvassa el az [Index attribútum hivatkozást](#bkmk_indexref) a következő szakaszban az attribútumok leírását.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Felhasználóiprofil-tulajdonság pontozási hivatkozás

**Megjegyzés:** A pontozási függvény csak szűrhető mezők vonatkozni.

| A tulajdonság | Leírás |
|----------|-------------|
| `name`   | Szükséges. Ez a pontozási a profil nevét. Egy mező azonos elnevezési szabályokat követi azt. Azt egy betűvel kell kezdődnie, nem tartalmazhat pontot, kettőspont vagy @ szimbólumokat, és nem indul el, (kis-és nagybetűket) "azureSearch" kifejezés. |
| `text` | A súlyok tulajdonságot tartalmazza. |
| `weights` | Nem kötelező. Arról, hogy egy mező neve és a relatív súly név – érték pár. Relatív súly egy pozitív egész vagy lebegőpontos szám kell lennie. Megadhatja, hogy a mező nevét a megfelelő súly nélkül. Súlyok, hogy egy mező egy másik viszonyított fontosságát szolgálnak. |
| `functions` | Nem kötelező. Figyelje meg, hogy egy pontozási függvény szűrhető mezők csak alkalmazhatók. |
| `type` | Függvények pontozási szükséges. Azt jelzi, hogy függvény használata a típusát. Az érvényes értékek `magnitude`, `freshness`, `distance` és `tag`. Az egyes pontozási profilok több függvényt is. A függvény neve kisbetű kell lennie. |
| `boost` | Függvények pontozási szükséges. Nyers pontszáma szorzójaként használt pozitív szám. Nem lehet egyenlő 1. |
| `fieldName` | Függvények pontozási szükséges. A pontozási függvény csak a mezőket, amelyek az index mező gyűjteményének részét, illetve szűrhető vonatkozni. Ezenkívül minden egyes függvény vezet be további korlátozásokkal (datetime mezővel, nagyságát egész szám, vagy dupla mezők, a távolságot a hely mezőben, és címkével karakterlánc vagy a karakterlánc-webhelycsoport mezők frissessége használt). Csak megadhatja egy függvény definition egyetlen mezőből. Például nagyságát kétszer használni a profillal, lenne szüksége egy, az egyes mezők két definíciótípus nagyságát felvenni. |
| `interpolation` | Függvények pontozási szükséges. Megadja a meredekség, amelynek a pontszám szolgáló lehetőségekről növeli a tartomány az elejétől a tartomány végére. Az érvényes értékek `linear` (alapértelmezett), `constant`, `quadratic`, és `logarithmic`. [Interpolations beállítása](#bkmk_interpolation) talál további információt. |
| `magnitude` | Függvény pontozási nagyságának változtatja meg egy numerikus mezőt az értékek alapján prioritású szolgál. A leggyakoribb használatát példák a következők:<ul><li>Minősítés csillagokkal: változtatja meg a pontozási belül a "Csillag minősítés" mező értéke alapján. Ha két elemek vonatkozó, az elemet a magasabb minősítéssel rendelkező először jelenik meg.</li><li>Margó: Két dokumentumok szempontjából fontos, amikor viszonteladótól merülnek fel újabb margók először rendelkező dokumentumok növelése.</li><li>Kattintson a megszámolja: nyomon alkalmazások kattintson a műveletek termék és-lapok keresztül, használhatja, amely a legtöbb forgalom első gyakran erősítése elemek nagyságát is.</li><li>Megszámolja letöltése: For applications követése letöltések, a függvény függvény lehetővé teszi, hogy növeli elem, amely a legtöbb letöltések van.</li></ul> |
| `magnitude:boostingRangeStart` | Beállítja a tartomány, amely fölött nagyságát elért hány kezdő értékét. Az érték egész vagy lebegőpontos szám kell lennie. Az 1-4-től csillagokkal Ez lehet 1. Margók 50 %-nál Ez az 50 lenne. |
| `magnitude:boostingRangeEnd` | Beállítja a tartomány, amely fölött nagyságát elért hány záró értéket. Az érték egész vagy lebegőpontos szám kell lennie. Az 1-4-től csillagokkal Ez lehet 4. |
| `magnitude:constantBoostBeyondRange` | Érvényes értékei igaz vagy hamis (alapértelmezett). Ha értéke igaz, a teljes erősítése továbbra is alkalmazhat, amelyeket kell egy értéknek a célmezőt, amely nagyobb, mint a tartomány felső végére. Értéke HAMIS, ha a célmezőt a tartományon kívül eső értéket rendelkező dokumentumok a erősítése függvény nem alkalmazható. |
| `freshness` | A pontozási függvény frissessége módosíthatja a rangsorolást értékek DateTimeOffset mezők értékein alapuló elemek szolgál. Ha például legújabb dátumú elem is kell rangsorban nagyobb, mint a régebbi elemek. (Tájékoztatjuk, hogy az is lehetséges sorszám elemekhez, például a jövőbeli dátumokhoz naptáresemények, hogy az elemek közelebbi bemutató is lehet rangsorban magasabb, mint az elemek további a jövőben.) Az aktuális programcsomag szolgáltatás a tartomány egyik végét az aktuális idő kijavítjuk. A másik végét az alapján múltbeli időpontot a `boostingDuration`. Későbbi időpontban tartomány növelése hozhatja létre a negatív `boostingDuration`. A pontozási profil alkalmazott, amelyen a szolgáló lehetőségekről a legfeljebb állapotra változik, és a legkisebb tartomány a interpolációval határozza meg a ráta (lásd az alábbi ábrán). Alkalmazott teljesítményfokozó tényező megfordításához válassza az 1-nél kisebb erősítése most. |
| `freshness:boostingDuration` | A beállítása után, amelyen a kiegyenlítés érvényességi időszakának egy adott dokumentum leáll. Lásd: [beállítása boostingDuration] [#bkmk_boostdur] szintaxis és példa a következő szakaszban. |
| `distance` | A pontozási függvényt használja hatással távolságot a pontszám alapján, hogy miként dokumentumok bezárása vagy távol viszonyított egy hivatkozás földrajzi hely. A hivatkozás helyét a paraméteres lekérdezés részeként számítható (használata a `scoringParameter` paraméteres lekérdezés), a maga lat argumentum. |
| `distance:referencePointParameter` | A hivatkozási hely használata lekérdezésekben átadandó paraméter. scoringParameter lekérdezési paraméter értéke. Lásd: [Dokumentumok keresése](search-api-2015-02-28-preview.md#SearchDocs) a lekérdezés paraméterei leírását. |
| `distance:boostingDistance` | Egy szám, amely jelzi a távolságot a kilométer a hivatkozás helyről, ahol teljesítményfokozó tartomány véget ér. |
| `tag` | A pontozási függvény címke hatással lévő pontszámhoz a dokumentumok a dokumentumok és a keresési lekérdezések címkék alapján használják. Dokumentumok közös a keresési lekérdezést címkék rendelkező fog kell csillapítja. A címkéket a keresési lekérdezés az egyes keresési kérelem pontozási paraméterként megadva (használata a `scoringParameter` paraméteres lekérdezés). |
| `tag:tagsParameter` | Adjon meg egy adott kérés címkék lekérdezések átadandó paraméter. `scoringParameter`az egyik lekérdezési paraméter. Lásd: [Dokumentumok keresése](search-api-2015-02-28-preview.md#SearchDocs) a lekérdezés paraméterei leírását. |
| `functionAggregation` | Nem kötelező. Hatókör, csak ha függvények vannak megadva. Az érvényes értékek: `sum` (alapértelmezett), `average`, `minimum`, `maximum`, és `firstMatching`. A keresési eredmény argumentum egyetlen érték, több változót, többek között a több függvényt számítja ki. A attribútumok azt jelzi, hogy hogyan az összes funkció növekedhet egyesítve egyetlen összesítő erősítése, majd a Alapdokumentum pontszám alkalmazott. A pontszám tf-idf értékét számítja ki a dokumentumot, és a keresési lekérdezés az alapul. |
| `defaultScoringProfile` | Végrehajtásakor a keresési kérelem, ha nincs pontozási profil van megadva, akkor alapértelmezett pontozási nem használt (tf-idf csak). Profilnév pontozási alapértelmezett beállítható, hogy ebben az esetben Azure keresési profilt szeretné használni, hogy nincs konkrét profil meg van adva a keresési kérelem okoz. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Interpolations beállítása

Interpolations lehetővé teszi a meredekség meghatározása, amelyhez a pontszám szolgáló lehetőségekről növeli a tartomány az elejétől a tartomány végére. A következő interpolations használható:

  - `Linear`: Az olyan elemeket, amelyek a max, min és tartományon belül a erősítése alkalmazza az elem egy folyamatosan csökkenő összeg történik. Lineáris van az alapértelmezett közbeszúrási pontozási profil.
  - `Constant`: Az olyan elemeket, amelyek a kezdési és befejezési tartomány belül a rangsorolt találatok egy állandó erősítése fog vonatkozni.
  - `Quadratic`: Az összehasonlítás, amely tartalmaz egy folyamatosan csökkenő erősítése Lineáris közbeszúrás másodfokú kezdetben csökkenni fog kisebb azt szeretné, és majd, akkor megközelíti a befejezési tartományban, így csökken sokkal nagyobb időközre. Közbeszúrási beállítás nem engedélyezett funkciók pontozási címkébe.
  - `Logarithmic`: Az összehasonlítás, amely tartalmaz egy folyamatosan csökkenő erősítése Lineáris közbeszúrás logaritmikus kezdetben csökkenni fog magasabb azt szeretné, és majd, akkor megközelíti a befejezési tartományban, így csökken a sok kisebb időközönként. Közbeszúrási beállítás nem engedélyezett funkciók pontozási címkébe.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>BoostingDuration beállítása

`boostingDuration`a frissessége függvény attribútum van. Az elévülési beállítása a segítségével pont után, amelyen a kiegyenlítés leáll egy adott dokumentumhoz. Például termék vonal vagy arculatának növelése 10 napos promóciós időszakra vonatkozóan, hogy meg ezeket a dokumentumokat a "P10D" a 10 napos időszak szeretne adni. Vagy növeli a következő hét közelgő eseményeket adja meg a "-P7D".

`boostingDuration`XSD "dayTimeDuration" értelmezi (korlátozott részhalmazát az ISO 8601 időtartam érték) lesz formázva. Ezt a minta: `[-]P[nD][T[nH][nM][nS]]`.

Az alábbi táblázat néhány példával szemlélteti.

| Időtartam | boostingDuration |
|----------|------------------|
| 1 nap | "P1D" |
| a 2 nappal és 12 órával | "P2DT12H" |
| 15 percet | "PT15M" |
| 30 nap, 5 óra, 10 perc, és 6.334 másodperc | "P30DT5H10M6.334S" |

További példák [XML-séma: adattípusok (W3.org webhely)](http://www.w3.org/TR/xmlschema11-2/).

**Lásd még:**
[Azure keresési szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) MSDN webhelyen <br/>
[Index (Azure keresés API-val) létrehozása](http://msdn.microsoft.com/library/azure/dn798941.aspx) az MSDN webhelyen<br/>
[A keresési indexhez pontozási profil hozzáadása](http://msdn.microsoft.com/library/azure/dn798928.aspx) MSDN webhelyen<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
