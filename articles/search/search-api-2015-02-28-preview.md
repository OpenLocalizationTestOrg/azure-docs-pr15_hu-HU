<properties
   pageTitle="Azure keresési szolgáltatás REST API-verzió 2015-02-28-előnézete |} Microsoft Azure |} Azure keresési előnézete API"
   description="Azure keresési szolgáltatás REST API-verzió 2015-02-28-előnézete kísérleti funkciókat, például a természetes nyelvi gázelemzők és moreLikeThis keresések tartalmazza."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure keresési szolgáltatás REST API: Verzió 2015-02-28 – előzetes verzió

Ez a témakör az összefoglaló dokumentációjában találhat `api-version=2015-02-28-Preview`. Ebben a villámnézetben kiterjeszti az aktuális általában elérhető verzió [api-verzió = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), mert az alábbi kísérleti funkciók:

- `moreLikeThis`a [Dokumentumok keresése](#SearchDocs) API paraméteres lekérdezés. Más dokumentumok, amelyek megfelelnek a megadott egy másik dokumentumba találja meg.

Néhány további részeinek a `2015-02-28-Preview` REST API dokumentálni külön-külön kell. Ezek a következők:

- [A pontozási profilok](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexelő](search-api-indexers-2015-02-28-preview.md)

Azure keresési szolgáltatás több verzióiban érhető el. További információ, olvassa el a [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

## <a name="apis-in-this-document"></a>API-hoz a dokumentumban

Azure keresési szolgáltatás API támogatja a két URL-cím szintaxis API-műveletek: egyszerű, és az OData (lásd: további információt [Az OData-(Azure keresési API) támogatási](http://msdn.microsoft.com/library/azure/dn798932.aspx) ). Az alábbi lista bemutatja az egyszerű szintaxisát.

[Index létrehozása](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Tárgymutató frissítése](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Index beszerzése](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Indexek listázása](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Index statisztika beszerzése](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Tesztelje Analyzer](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Index törlése](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Hozzáadása, törlése, és Index adatainak frissítése](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Dokumentumok keresése](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[A keresés dokumentum](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Dokumentumok száma](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Javaslatok](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Tárgymutató-műveletek

Létrehozhat és Azure keresőszolgáltatás egyszerű HTTP-kérések (BEJEGYZÉST, GET, helyezése, Törlés) egy adott indexben erőforrás szemben keresztül az Indexek kezelése. Ha tárgymutatót szeretne készíteni, először KÜLD egy JSON dokumentumot, amely leírja a tárgymutató-séma. A séma a mezőket az indexet, az adattípusok és hogyan használhatók (például a teljes szöveges kereséseket, szűrők, rendezés és faceting) definiálja. Megadja is pontozási profilokat, suggesters és más jellemzők konfigurálása az index viselkedését.

Az alábbi példa a szállások információt keres, a Leírás mezőben megadott két különböző nyelven használható séma ábrája biztosít. Figyelje meg, hogyan attribútumok használatának szabályozása a mezőt. Példa a `hotelId` lesz a dokumentum kulcs (`"key": true`), és nem szerepel a teljes szöveges keresés (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Az index létrehozása után a dokumentumok feltöltése a tárgymutató tölti fel. Lásd: a [Hozzáadás vagy a frissítés dokumentumokat](#AddOrUpdateDocuments) a következő lépésben.

Az Azure keresési indexelés – videó Bevezetés az [Azure Search csatorna 9 felhő terjed ki rész](http://go.microsoft.com/fwlink/p/?LinkId=511509)című témakör tartalmaz.


<a name="CreateIndex"></a>
## <a name="create-index"></a>Index létrehozása

Index az elsődleges eszközök rendszerezés és keresés a dokumentumok Azure keresés, miként a táblázat egy adatbázis rekordjainak rendezi hasonlít. Minden index, hogy az összes felel meg az index sémának (mezőnevek, adattípusok és tulajdonságok), de az indexek is adhat meg a további szerkezetek (suggesters pontozási profilok és CORS beállítások) más keresési viselkedések meghatározó dokumentumgyűjtemények tartalmaz.

Létrehozhat új index létrehozása az Azure keresési szolgáltatást a HTTP-bejegyzés vagy helyezése felkérés belül. A kérés törzsében, amely meghatározza az index és a konfigurációs információk JSON séma.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Azt is megteheti helyezése használja, és adja meg az index neve a URI. Ha az index nem létezik, létrejön.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Index létrehozása határozza meg, hogy a dokumentumok tárolása és keresési műveletek használt szerkezete. Az index feltöltése egy olyan külön művelet. Az ebben a lépésben az [Indexelő](https://msdn.microsoft.com/library/azure/mt183328.aspx) (a támogatott adatforrások érhető el) és a művelet [hozzáadása, frissítése, vagy törölje dokumentumok](https://msdn.microsoft.com/library/azure/dn798930.aspx) is használhatja. A fordított index jön létre, amikor a rendszer kézbesíti a dokumentumokat.

**Megjegyzés**: az indexek engedélyezett maximális száma réteg árak változik. Az ingyenes szolgáltatás lehetővé teszi, hogy legfeljebb 3 indexek. Szokásos szolgáltatás lehetővé teszi, hogy a keresési szolgáltatás használati 50 indexek. [Korlátozások és korlátozásokkal](http://msdn.microsoft.com/library/azure/dn798934.aspx) talál részleteket.

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmet. A **Create Index** kérés is alakítható ki egy BEJEGYZÉST, vagy a helyezése módszerrel. BEJEGYZÉS használatakor, meg kell adnia egy index neve együtt index sémadefiníciója összehívás törzsébe. A helyezése az index neve része URL-CÍMÉT. Ha az index nem létezik, akkor jön létre. Ha már létezik, az új definíció frissül.

Az index neve kell kell kisbetű, kezdje egy betűt vagy számot, nincs perjellel vagy pontot és lehet kisebb, mint a 128 karaktert. Után az index neve kezdve egy betűt vagy számot, a nevet a többi is elhelyezhet bármely betű, a szám és a szaggatott vonal, mindaddig, amíg a szaggatott vonal nem egymást követő állnak.

A `api-version` szükség. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) rendelkezésre álló verziók listáját.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `Content-Type`: Kötelező. Beállítani`application/json`
- `api-key`: Kötelező. A `api-key` használt
- az értekezlet-összehívást a keresési szolgáltatás hitelesíteni. Érték karakterlánc, egyedi a szolgáltatásban. A **Create Index** kérelem tartalmaznia kell egy `api-key` fejlécben a rendszergazda kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

<a name="RequestData"></a>
**Összehívás törzsébe szintaxisa**

A kérés törzsében sémadefiníciója, amelyek tartalmazzák még a álló adatokat fog kell géppel be ezt az indexet, adattípusok, attribútumok, valamint választható listáját, amelyekkel megfelelő dokumentumok pontszám lekérdezés időben profilok pontozási dokumentumokból.

Ne feledje, hogy a bejegyzés kérése, meg kell adnia az index neve összehívás törzsébe.

Csak lehet egy kulcsmező a tárgymutató. Lehet karakterlánc mező le. Ez a mező minden dokumentum, az index tárolva egyedi azonosítója jelöl.

A tárgymutató fő részei a következők:

- `name`
- `fields`amely az indexhez legördülő menüjéből többek között a nevét, az adattípus és a Tulajdonságok ezt a mezőt a megengedett műveletek meghatározó géppel lesz.
- `suggesters`automatikus kiegészítési vagy javaslatokat lekérdezéseket használni.
- `scoringProfiles`Egyéni keresési eredmény rangsorolási használható. További információ a [Hozzáadás pontozási profilok](https://msdn.microsoft.com/library/azure/dn798928.aspx) című témakör tartalmaz.
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` hogyan a dokumentumok lekérdezések osztani példánycímkének/kereshető tokenek használta. [Azure keresés Analysis](https://aka.ms//azsanalysis) talál részleteket.
- `defaultScoringProfile`írja felül az alapértelmezett viselkedés pontozási szolgál.
- `corsOptions`Ahhoz, hogy a tárgymutatóhoz határokon származási lekérdezéseket.

A kérés tartalom tagolásának szintaxisa az alábbi képlettel történik. Egy minta kérelmet a ebben a témakörben további megadva.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Index attribútumok**

Index létrehozása esetén a következő attribútumok állítható be. Pontozási és pontozási profilokkal kapcsolatos részletekért olvassa el a [pontozási profilok hozzáadása](https://msdn.microsoft.com/library/azure/dn798928.aspx)című témakört.

`name`-Állítja be annak a mezőnek a nevét.

`type`-A mező adattípusa állítja be. Lásd: [Támogatott adattípusok](#DataTypes) listáját a támogatott típusok.

`searchable`-Megjelöléssel látja el a mező teljes szöveges keresési tudja. Ez azt jelenti, például a word aktuális indexelés során analysis halad át. Ha egy `searchable` mezőt egy érték, például "sütni day", akkor belső fog lehet osztani az egyes tokenek "sütni" és "nap". Ez a teljes szöveges megkeresi a jelen feltételek lehetővé teszi. Típusú mezők `Edm.String` vagy `Collection(Edm.String)` vannak `searchable` alapértelmezés szerint. Más típusú mezők nem lehet `searchable`.

  - **Megjegyzés**: `searchable` mezők felhasználása plusz térközt a index, mivel az Azure keresési tárolja a mező értékét a teljes szöveges keresés további tokenekre verzióját. Ha így helyet takaríthat meg az index és kiterjedjen felvenni kívánt mező nem szükséges, beállítása `searchable` való `false`.

`filterable`-Lehetővé teszi a mező a hivatkozni kívánt `$filter` lekérdezések. `filterable`eltér `searchable` a karakterláncok kezelésének módját. Típusú mezők `Edm.String` vagy `Collection(Edm.String)` , amelyek `filterable` nem kerülnek word törhető, így összehasonlításokat csak pontosan egyező van. Ha például beállíthatja, hogy ilyen mező `f` "sütni napra," `$filter=f eq 'sunny'` nincs találat, kifejezésre keresve, de `$filter=f eq 'sunny day'` lesz. Minden mező `filterable` alapértelmezés szerint.

`sortable`-Alapértelmezés szerint a rendszer találatok rendezése pontszámhoz, de sok élményt a felhasználóknak is szükség van a dokumentumok mező szerint rendezheti. Típusú mezők `Collection(Edm.String)` nem lehet `sortable`. Minden mező `sortable` alapértelmezés szerint.

`facetable`-Jellemzően a keresési eredmények bemutató, amely tartalmazza a találati száma szerint kategóriát (például keresési digitális fényképezőgépek és lásd: a találatok arculatának, Megapixel, ár, stb.). Nem lehet használni ezt a beállítást típusú mezők `Edm.GeographyPoint`. Minden mező `facetable` alapértelmezés szerint.

  - **Megjegyzés**: típusú mezők `Edm.String` , amelyek `filterable`, `sortable`, vagy `facetable` lehet legfeljebb 32 KB hossza. Ennek az oka, hogy ilyen mező számít egy kifejezést, és az Azure keresési kifejezés maximális hossza 32KB. Ha egyetlen karakterlánc mezőben tárolja ennél több szöveget szeretne, akkor kell explicit módon beállított `filterable`, `sortable`, és `facetable` való `false` a tárgymutató-definícióban.

  - **Megjegyzés**: a mező rendelkezik-e meg a fenti attribútumok egyike `true` (`searchable`, `filterable`, `sortable`, vagy`facetable`) a mező hatékony zárni az, fordított index. Ez a beállítás akkor hasznos, ha mezőket, amelyeket nem használják a lekérdezésekben, de a keresési eredmények között van szükség. Az index ilyen mező kizárása javítja a teljesítményt.

`key`-Jelöli meg a dokumentumok belül az index egyedi azonosítókat tartalmazó mezőt. Pontosan egy mező szerint kell választani a `key` típusú mező, és kell lennie `Edm.String`. Kulcsát lépések elvégzésével keresheti meg a dokumentumokat közvetlenül a [Keresési API](#LookupAPI)keresztül használható.

`retrievable`-Állítja be, hogy a mező a keresési eredmény adhatók vissza.  Ez akkor hasznos, ha szeretné használni (például margó) mező szűrőként, rendezés és mechanizmusa pontozási, de nem szeretné, hogy látható legyen a végfelhasználói a mezőt. Az attribútum kell `true` az `key` mezőket.

`analyzer`-Állítja be a nevét a Keresés és indexelés idejét a mező használata a analyzer. Az engedélyezett értékhalmaz [gázelemzők](https://msdn.microsoft.com/library/mt605304.aspx)című témakör tartalmaz. Ezt a lehetőséget kínál csak `searchable` együtt vagy mezőket, és nem állíthatók `searchAnalyzer` vagy `indexAnalyzer`.  Miután a analyzer választja, akkor a mező nem módosíthatók.

`searchAnalyzer`-Állítja be a analyzer használt keresési időben a mező nevére. Az engedélyezett értékhalmaz [gázelemzők](https://msdn.microsoft.com/library/mt605304.aspx)című témakör tartalmaz. Ezt a lehetőséget kínál csak `searchable` mezőket. Meg kell együtt `indexAnalyzer` együtt nem állítható be a `analyzer` lehetőséget. A analyzer frissíthető meglévő mezőre.

`indexAnalyzer`-Állítja be a analyzer használt indexelési időben a mező nevére. Az engedélyezett értékhalmaz [gázelemzők](https://msdn.microsoft.com/library/mt605304.aspx)című témakör tartalmaz. Ezt a lehetőséget kínál csak `searchable` mezőket. Meg kell együtt `searchAnalyzer` együtt nem állítható be a `analyzer` lehetőséget. Miután a analyzer választja, akkor a mező nem módosíthatók.

`suggesters`-Állítja be a Keresés mód és a vonatkozó ajánlások a tartalom forrása mezőt. [Suggesters](#Suggesters) talál további információt.

`scoringProfiles`-Egyéni pontozási viselkedések, amelyekkel befolyásoló mely elemek jelenjenek meg a keresési eredmények között magasabb határozza meg. Pontozási profilok mező súlyok és a függvények épülnek fel. Lásd: a [profilok pontozási hozzáadása](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt a használt pontozási profil tulajdonságait.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Nyelvek támogatása**

Kereshető mezők analysis esnek át, hogy a legtöbb gyakran tényezői: word aktuális, a szöveg normalizálás és a kifejezések kiszűrésével. Alapértelmezés szerint az Azure keresési kereshető mezők az alábbi["Unicode szöveg szegmens"](http://unicode.org/reports/tr29/) szabályok elemek oszlik a szöveg [Apache Lucene szabványos analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) elemzése. Emellett a szabványos analyzer alakítja át az összes karakter kisbetű formájukban. Indexelt dokumentumokat és a keresési kifejezések lépjen az elemzés keresztül az indexelés és a lekérdezés feldolgozása során.

Azure keresési támogatja a különféle nyelvek. Minden egyes nyelvi egy szabványos szöveget analyzer, amelyek adott nyelvre jellemzői számlák szükséges. Azure keresési kétféle típusú gázelemzők:

- 35 gázelemzők Lucene mögött.
- 50 gázelemzők mögött a saját Microsoft természetes nyelvi technológiát használ az Office vagy a Bing feldolgozása.

Néhány fejlesztők előfordulhat, hogy inkább a Lucene ismerősebb egyszerű, Megnyitás-forrás megoldás. Lucene gázelemzők gyorsabb, de a Microsoft gázelemzőknek speciális funkciókat, például Lemmatizálás, a word decompounding (például német, dán, holland, svéd, norvég, észt, Befejezés, magyar, Szlovák nyelven) és a szervezet felismerés (URL-címeit, e-mailek, dátumok, számok). Ha lehetséges futtatnia kell a Microsoft és a Lucene gázelemzők döntse el, mely még jobban megfelelnek összehasonlítása.

***Hogyan összehasonlítása***

Az angol a Lucene analyzer a szabványos analyzer kiterjed. Azt eltávolítja possessives (záró meg) a szavakat, szerinti [Porter származó algoritmus](http://tartarus.org/~martin/PorterStemmer/)származó vonatkozik, valamint angol [szavak leállítása](http://en.wikipedia.org/wiki/Stop_words).

Összehasonlító, az a Microsoft analyzer helyett, amelyek Lemmatizálás hajt végre. Akkor is képes kezelni ragozott és szabálytalan word-űrlapok sokkal jobban milyen eredményekre több megfelelő találatok (megtekintés modul 7 [Azure keresési MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) bemutató további részleteket).

Microsoft gázelemzők indexelés átlagosan háromszor két lassabb, mint a Lucene egyenértékű funkcióiról, attól függően, hogy a nyelv van. A keresés teljesítményével nem átlagos mérete lekérdezések jelentősen hatással lehet.

***Konfiguráció***

A tárgymutató-definíció minden egyes mezőhöz, beállíthatja, hogy a `analyzer` tulajdonság egy arról, hogy melyik nyelvi és szállító analyzer nevét. Az azonos analyzer fog vonatkozni, amikor az indexelés és a Keresés mező.
Ha például angol, francia vagy spanyol megtalálható az egymás melletti ugyanazt az indexet a szállások leírások külön mezőinek is. A ["searchFields" lekérdezési paraméter](#SearchQueryParameters) segítségével megadhatja, melyik nyelvspecifikus mező Keresés szemben, a lekérdezések a. Lekérdezés példákat tartalmazó áttekintheti a `analyzer` tulajdonság a [Dokumentumok keresése](#SearchDocs). 

***Analyzer lista***

Az alábbi van együtt Lucene és a Microsoft analyzer nevek támogatott nyelveinek listáját.

<table style="font-size:12">
    <tr>
        <th>Nyelvi</th>
        <th>Microsoft analyzer neve</th>
        <th>Lucene analyzer neve</th>
    </tr>
    <tr>
        <td>arab</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>örmény</td>
        <td></td>
        <td>HY.lucene</td>
    </tr>
    <tr>
        <td>Bangla</td>
        <td>BN.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>baszk</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>bolgár</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>katalán</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
    </tr>
    <tr>
        <td>kínai (egyszerűsített)</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>     
    </tr>
    <tr>
        <td>kínai (hagyományos)</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>     
    <tr>
    <tr>
        <td>horvát</td>
        <td>hr.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>cseh</td>
        <td>cs.Microsoft</td>
        <td>cs.lucene</td>      
    </tr>    
    <tr>
        <td>dán</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>holland</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>  
    </tr>    
    <tr>
        <td>angol</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>észt</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>finn</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>francia</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>galíciai</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>német</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>görög</td>
        <td>el.Microsoft</td>
        <td>el.lucene</td>      
    </tr>
    <tr>
        <td>gudzsaráti</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>héber</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>      
    </tr>
    <tr>
        <td>magyar</td>      
        <td>hu.Microsoft</td>
        <td>hu.lucene</td>
    </tr>
    <tr>
        <td>izlandi</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonéz (idő szerint)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>ír</td>
        <td></td>
        <td>GA.lucene</td>
    </tr>
    <tr>
        <td>olasz</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>japán</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>kannada</td>
        <td>ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>koreai</td>
        <td>ko.Microsoft</td>
        <td>ko.lucene</td>
    </tr>
    <tr>
        <td>lett</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>litván</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>malajálam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maláj (latin betűs)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>marathi</td>
        <td>MR.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>norvég</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>perzsa</td>
        <td></td>
        <td>fa.lucene</td>      
    </tr>
    <tr>
        <td>lengyel</td>
        <td>pl.Microsoft</td>
        <td>pl.lucene</td>      
    </tr>
    <tr>
        <td>Portugál (brazíliai)</td>
        <td>PT-Br.microsoft</td>
        <td>PT-Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugál (portugáliai)</td>
        <td>PT-Pt.microsoft</td>        
        <td>PT-Pt.lucene</td>
    </tr>
    <tr>
        <td>pandzsábi</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>román</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>orosz</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>  
    </tr>
    <tr>
        <td>szerb (cirill betűs)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Szerb (latin betűs)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>szlovák</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>szlovén</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>spanyol</td>
        <td>es.Microsoft</td>
        <td>es.lucene</td>
    </tr>
    <tr>
        <td>svéd</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>telugu</td>
        <td>Te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>thai</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>török</td>
        <td>tr.Microsoft</td>
        <td>tr.lucene</td>      
    </tr>
    <tr>
        <td>ukrán</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>vietnami</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Továbbá Azure keresési nyújt a nyelv – felismerése nélkül analyzer konfigurációk</td>
    <tr>
        <td>A szabványos ASCII Hajtogatásának</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode szöveg szegmens (normál tokenizáló)</li>
            <li>ASCII-hajtás szűrő - Unicode-karakterek, amelyek nem tartoznak az első 127 ASCII-karakterek be saját ASCII egymásnak megfelelő elemei alakítja át. Ez akkor hasznos mellékjelek eltávolíthat.</li>
        </ul>
        </td>
    </tr>
</table>

<i>Lucene</i> attribútummal névvel rendelkező összes gázelemzők [Apache Lucene nyelvi gázelemzők](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html)vannak hajtott. További információt a szűrő hajtogatásának ASCII megtalálható [Itt](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` határozza meg, hogy mely mezők indexet használatával támogatja az automatikus kiegészítési kiterjedjen. A szokásos részleges keresést karakterláncok elküldi a [Javaslatok API](#Suggestions) , miközben a felhasználó az írja be a keresési lekérdezést, majd a javasolt kifejezések lásson az API-t. Egy suggester, amely csak a tárgymutató határozza meg, hogy mely mezők használatával összeállítása a javaslatokat megjelenítő keresés kifejezéseket. Lásd: [Suggesters](#Suggesters) konfigurációs részleteket.

**A pontozási profilok**

A `scoringProfile` határozza meg az egyéni pontozási viselkedések, amelyekkel befolyásoló milyen elemek jelennek meg a nagyobb a keresési eredmények között. Pontozási profilok mező súlyok és a függvények épülnek fel. Őket használni, akkor adja meg a profil nevére a lekérdezési karakterlánc.

Alapértelmezett profil pontozási számítja ki egy keresési eredmény az eredményhalmaz minden elemet a színfalak mögött működik. Használhatja a belső név nélküli pontozási profilt. Másik lehetőségként beállítása `defaultScoringProfile` profilt szeretné használni egy egyéni az alapértelmezett, valahányszor a lekérdezési karakterlánc nem szerepel a saját profil meghívott.

További információt a [hozzáadása pontozási profilok a keresési indexhez (Azure keresési szolgáltatás REST API -val)](search-api-scoring-profiles-2015-02-28-preview.md) című témakör tartalmaz.

**CORS beállításai**

Ügyféloldali Javascript nem hívja bármely API-khoz alapértelmezés szerint, mert megakadályozza, hogy a böngészőben az összes határokon származási kérelem. (Határokon származási erőforrás-megosztás) CORS módosításával engedélyezheti a `corsOptions` attribútum határokon származási lekérdezések az indexelendő engedélyezése. Megjegyzés: az API-khoz támogatási CORS biztonsági okokból csak lekérdezés. A következő beállításokat adhatja meg a CORS:

- `allowedOrigins`(kötelező): Ez az, hogy megkapja a hozzáférést a tárgymutatóhoz forrásokból listáját. Ez azt jelenti, hogy az adott forrásokból felszolgált bármely Javascript-kód lesz jogosult a lekérdezés az index (feltételezve, hogy a megfelelő API kulcsot biztosít). Minden származás általában az űrlap van `protocol://fully-qualified-domain-name:port` a port gyakran ugyan. [Ez a cikk](http://go.microsoft.com/fwlink/?LinkId=330822) további információt találhat.
 - Ha azt szeretné, az összes forrásokból való hozzáférés engedélyezése, tartalmazza a `*` egyetlen elemként a `allowedOrigins` tömb. Megjegyzendő, hogy **Ez nem ajánlott gyakorlás gyártási keresési szolgáltatásokhoz.** De hasznos lehet fejlesztés és hibakeresés céljából.
- `maxAgeInSeconds`(nem kötelező): böngészők használatával ezt az értéket határozza meg az időtartam (másodpercben) gyorsítótár CORS ellenőrzés válaszokat. A nem negatív egész számok ennek kell lennie. Minél nagyobb értéket, a jobb teljesítmény lesz, de a hosszabb időt vesz igénybe CORS házirend-módosítások érvénybe léptetéséhez. Ha nem állít be, az 5 perc alapértelmezett időtartamot lesz.

<a name="CreateUpdateIndexExample"></a>
**Példa a szervezet kérése**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Válasz**

A sikeres kérésekben: "201 létrehozva".

Alapértelmezés szerint a válasz szervezet létrehozott index meghatározása a JSON fog tartalmazni. Ha a `Prefer` kérés fejléce értéke `return=minimal`, a válasz szervezet üres lesz, és a sikeres állapotkód "204 Nincs tartalom" helyett "201 létrehozva". Az IGAZ, függetlenül attól, hogy helyezése vagy a bejegyzés használt létre szeretné hozni az indexet.

**Megjegyzések:**

Jelenleg korlátozott támogatás index séma frissítéseit. Lenne szükség, például mezőtípusok újraindexelés séma frissítéseiről jelenleg nem támogatott. Bár a mezők nem módosíthatók vagy törölt, az új mezőket is hozzá kell adni egy meglévő indexet bármikor. Új mező felvétele után a meglévő dokumentumokat a tárgymutató automatikusan rendelkeznek az adott mező null értéket. Nincsenek további tárterület mindaddig, amíg az új dokumentum bekerülnek az indexhez fogyni fog.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Azure keresési javaslatok funkció egy javaslatokat vagy az automatikus kiegészítési lekérdezés képesség, a potenciális keresési kifejezések szót a Keresés mezőbe beírt részleges karakterlánc bemenetben válaszul listájával. Akkor bizonyára észrevette lekérdezési javaslatok kereskedelmi keresőmotorok használata esetén: feltételeinek listáját ".NET" beírja a Bing eredménye ".NET 4,5", ".NET-keretrendszer 3.5-ös", és így tovább. Amikor a keresési szolgáltatás REST API-t, javaslatok végrehajtása egy egyéni Azure keresőalkalmazás az alábbiakra van szükség:

- Javaslatok engedélyezéséhez hozzáadásával egy **suggester** építés a index, a nevének, a Keresés mód és a mezők, amelynek javaslatokat meghívott listája megadása. Például ha ad meg "város" mezőként egy adatforrás, írja be a "Tengeri" Keresés részleges karakterlánc eredményezi "Seattle", "Tengerpart" és "Seatac" (az összes közül három tényleges város neve) amely, a lekérdezési javaslatok a felhasználó számára.

- Javaslatok meghívásához a [Javaslatok API](#Suggestions) bekapcsolódhat az alkalmazás kódját. A szokásos részleges keresést karakterláncok küldi el a szolgáltatás, miközben a felhasználó az írja be a keresési lekérdezést, majd a javasolt kifejezések lásson Ez az API.

Ez a cikk ismerteti, hogyan egy **suggester**konfigurálása. A [Javaslatok API](#Suggestions) hogyan használják a suggester kapcsolatos részletekért tekintse meg.

**Használat**

`Suggesters`jönnek létre az index és a munkavégzés a legjobban, ha a használt egyedi dokumentumokat, hanem laza kifejezéseket vagy kifejezések alapján. A legjobb jelölt mezők, címek, nevek és egyéb elem azonosítani tudja viszonylag rövid kifejezéseket. Kisebb hatékonyak ismétlődő, például címkéket, és a kategóriák és nagyon sok mezők például leírása és a Megjegyzések mezők.

A tárgymutató-definíció részeként is hozzáadhat egy egyetlen suggester, hogy a `suggesters` webhelycsoport. Egy suggester meghatározó tulajdonságok az alábbiak:

- `name`: A suggester a neve. A suggester neve használhatja, ha a hívás a `suggest` API-val.
- `searchMode`: Stratégia jelölt mondatok kereséséhez használja. A jelenleg támogatott csak mód `analyzingInfixMatching`, amely kifejezések elején vagy mondatot közepén rugalmas egyező hajt végre.
- `sourceFields`: Egy vagy több olyan mezők, amelyek az javaslatok a tartalom forrása egy listája. Típusú mezők `Edm.String` és `Collection(Edm.String)` javaslatok az forrásai lehetnek. Csak olyan mezők, amelyeknek egy egyéni nyelvi analyzer beállítása nem használhatók.

**Példa suggester**

Egy suggester az index része. Csak egy suggester is megtalálható a `suggesters` a jelenlegi verzióval, a mezők gyűjtemény mellett a gyűjtemény és `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Ha korábban Azure Keresés nyilvános előzetes verzióját `suggesters` lecseréli egy régebbi logikai tulajdonság (`"suggestions": false`), amely csak a támogatott előtag javaslatok rövid karakterláncok (3-25 karakterből). A csere, `suggesters`, támogatja az összefűzési egyező által talált hibákat keresési karakterláncot az jobb hibatűrést a megfelelő kifejezéseket elején vagy közepén mező tartalmát,. Általában elérhető megjelenési kezdve ez ettől kezdve a javaslatok API csak végrehajtását. A régebbi `suggestions` tulajdonságot, amelyet a jelent meg `api-version=2014-07-31-Preview` is működő ebben a verzióban, de nem működik a a `2015-02-28` vagy újabb verziók Azure keresés.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Tárgymutató frissítése

Egy meglévő indexet belül Azure keresési HTTP ELHELYEZNI felkérés használatával módosíthatja. Frissítések lehetnek az új mező felvétele a meglévő séma, CORS beállítások módosítása és pontozási profilok módosításával. További információt a [profilok pontozási hozzáadása](https://msdn.microsoft.com/library/azure/dn798928.aspx) talál. Az index frissítéséhez kattintson a kérelem URI nevének megadása:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Fontos:** Index séma frissítések támogatása korlátozódik, amely nincs szükség a keresési index Újraépítés műveletek. Lenne szükség újraindexelés, például típusú mező séma frissítéseiről jelenleg nem támogatott. Új mezők felvehetők bármikor, bár a mezők nem változott, és nem törölhető. Ugyanez igaz `suggesters`. Új mezők lehet adni szeretne egy időben suggester a mezők kerülnek, de a mezőket nem lehet eltávolítani `suggesters` és mezők nem adható hozzá `suggesters`.

Ha új mezőt szeretne index, a meglévő dokumentumokat a tárgymutató automatikusan rendelkeznek az adott mező null értéket. Nincsenek további tárterület mindaddig, amíg az új dokumentum bekerülnek az indexhez fogyni fog.

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmet. A **Frissítés Index** kérés összeállítás használatával HTTP HELYEZI el. A helyezése az index neve része URL-CÍMÉT. Ha az index nem létezik, akkor jön létre. Ha az index már létezik, az új definíció frissül.

Az index neve kell kell kisbetű, kezdje egy betűt vagy számot, nincs perjellel vagy pontot és lehet kisebb, mint a 128 karaktert. Után az index neve kezdve egy betűt vagy számot, a nevet a többi is elhelyezhet bármely betű, a szám és a szaggatott vonal, mindaddig, amíg a szaggatott vonal nem egymást követő állnak.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `Content-Type`: Kötelező. Beállítani`application/json`
- `api-key`: Kötelező. A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. A **Frissítés Index** kérelem tartalmaznia kell egy `api-key` fejlécben a rendszergazda kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Összehívás törzsébe szintaxisa**

Egy meglévő indexet frissítésekor a szervezet tartalmaznia kell az eredeti sémadefiníciója és az új mezőket hozzáadni, valamint a módosított pontozási profilok suggesters és CORS beállítások, ha van ilyen. A pontozási profilok és a CORS beállításokat nem módosítja, ha az index létrehozása esetén az eredeti szerepelnie kell. Általános frissítések használni kívánt legjobb minta GET az index meghatározása beolvasásához, módosítsa, majd frissítse azt helyezése.

A tárgymutató létrehozásához használt séma szintaxisa az alábbi minősítésű kényelmesebbé. [Create Index](#CreateIndex) talál további információt.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Válasz**

A sikeres kérésekben: "204 Nincs tartalom".

Alapértelmezés szerint a válasz szervezet üres lesz. Jó helyen jár Ha a `Prefer` kérés fejléce értéke `return=representation`, a válasz szervezet fogja tartalmazni a JSON frissült index meghatározáshoz. Ebben az esetben lesz a sikeres állapotkódja "200 OK".

**Egyéni gázelemzők index definíciója frissítése**

Miután egy analyzer, egy tokenizáló, jogkivonat szűrő vagy karakter szűrő van beállítva, nem lehet módosítani. Újakat is adható hozzá egy meglévő indexet, csak akkor, ha a `allowIndexDowntime` jelző értéke igaz, az index frissítés összehívásban: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Figyelje meg, hogy ez a művelet tesz a tárgymutatóhoz offline legalább néhány másodpercet, okoz az indexelés és a lekérdezés kérések sikertelen lesz. Lehet, hogy az index teljesítmény és az írási elérhetősége látók számára néhány percig, amíg az index frissítése után vagy hosszabb ideig nagyon nagy indexek.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Lista indexek

A **Lista indexek** műveletet adja vissza az indexek listájának jelenleg az Azure keresési szolgáltatás.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmet. A **Lista indexek** kérelem is alakítható ki a GET metódussal.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: Kötelező. A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. A **Lista indexek** kérelem tartalmaznia kell egy `api-key` állítsuk be egy rendszergazdai kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

Nincs lehetőség.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

Íme egy példa válasz szervezet:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Figyelje meg, hogy a választ, amely érdekli tulajdonságait le is szűrheti. Ha például ha azt szeretné, hogy csak a tárgymutató nevek listája, használja az OData `$select` lekérdezés lehetőséget:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

Ebben az esetben a fenti példa válasza tűnik, az alábbi képlettel történik:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Ez a sávszélesség mentheti, ha rendelkezik az indexek sok a keresési szolgáltatás egy hasznos technika.

<a name="GetIndex"></a>
## <a name="get-index"></a>Index beszerzése

Az **Első Index** művelet megszerzi Azure keresési index meghatározása.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Kérés**

HTTPS szükség a szolgáltatási kérelmeket. A **Tárgymutató Get** kérés is alakítható ki a GET metódussal.

A kérelem URI [index neve] Itt adhatja meg, mely index térni az indexek gyűjteményből.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. A **Tárgymutató Get** kérés tartalmaznia kell egy `api-key` állítsuk be egy rendszergazdai kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

Nincs lehetőség.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

Lásd: a példában az [létrehozása és frissítése az Index](#CreateUpdateIndexExample) egy példát a válasz tartalom JSON.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Index törlése

Az **Index törlése** művelet eltávolítja a Azure keresési szolgáltatás index és a velük társított dokumentumokat. Az index neve elérheti, az Azure-portálon szolgáltatás irányítópultról, vagy az API-t. [Lista indexek](#ListIndexes) talál további információt.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Kérés**

HTTPS szükség a szolgáltatási kérelmeket. **Index törlése** kérésére is alakítható ki a törlés módszerrel.

A kérelem URI [index neve] Itt adhatja meg, mely index törlése az indexek gyűjteményből.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: Kötelező. A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterláncot, a szolgáltatás URL-címre egyedi. **Index törlése** kérésére tartalmaznia kell egy `api-key` fejlécben a rendszergazda kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

Nincs lehetőség.

**Válasz**

Állapotkód: 204 Nincs tartalom beállítási jelez sikeres válaszra várni.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Index statisztika beszerzése

Az **Első Index statisztika** művelet Azure keresés az aktuális index, valamint a tárterület-használat a dokumentumok számát adja eredményül.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] A dokumentum darab és a tároló méretét statisztika begyűjtési minden néhány percet, nem a valós időben. Emiatt a statisztika, ez az API által visszaadott nem feltétlenül tükrözi módosítások legutóbbi indexelési művelet okozza.

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmek. Az **Index statisztika Get** kérés is alakítható ki a GET metódussal.

A kérelem URI [index neve] azt jelenti, hogy a szolgáltatást, hogy a megadott index index statisztikai visszatérési.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.


**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. Az **Index statisztika Get** kérés tartalmaznia kell egy `api-key` állítsuk be egy rendszergazdai kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

Nincs lehetőség.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

Válasz törzse a következő formátumban:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Tesztelje Analyzer

Az **Elemzés API** jeleníti meg, hogyan egy analyzer oszlik a szöveg tokenek.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmek. A **Elemzése API** kérés is alakítható ki a BEJEGYZÉST módszerrel.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.


**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. A **Elemzése API** kérés tartalmaznia kell egy `api-key` állítsuk be egy rendszergazdai kulcs (nem pedig a lekérdezés billentyűt).

Az index neve és a szolgáltatás neve a kérelem URL-címe összeállításához is lesz szüksége. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

vagy

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

A `analyzer_name`, `tokenizer_name`, `token_filter_name` és `char_filter_name` kell lennie, előre definiált vagy egyéni gázelemzők, tokenizers, jogkivonat szűrők és az index karakter szűrők érvényes nevét. További tudnivalók a folyamat lexikai elemzési információ [Analysis Azure keresés](https://aka.ms/azsanalysis).

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

Válasz törzse a következő formátumban:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Példa API elemzése**

**Kérés**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Válasz**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Dokumentum-műveletek

Azure keresési index tárolja a felhőben, és töltve JSON dokumentumok feltöltése a szolgáltatás használatával. A dokumentumok feltöltése a keresési adat corpus tartalmazzák. Dokumentumok mezőket, melyek vannak tokenekre bontott be keresési kifejezések, azok van feltöltve. tartalmaznak. A `/docs` URL-cím szakaszában az Azure keresési API jelöli az index dokumentumok gyűjteménye. Az összes művelet végrehajtását, például feltöltése a gyűjteményben, egyesítése, törlése és dokumentumok take lekérdezése helyezze egyetlen index létrehozása környezetében úgy URL-címét az ezek a műveletek mindig elkezdenek az `/indexes/[index name]/docs` egy adott indexben nevet.

Az alkalmazás kódot vagy generáljon JSON dokumentumok feltöltése a Azure keresési vagy dokumentumok betöltése, ha az adatforrás Azure SQL-adatbázis vagy DocumentDB az [Indexelő](https://msdn.microsoft.com/library/dn946891.aspx) is használhatja. Indexek bekerülnek általában az Ön által egyetlen adatkészletet.

Meg kell terveznie, a rendelkező minden elemhez, amelyben keresni szeretne egy dokumentumot. A movie bérleti alkalmazások lehet filmet egy dokumentumot, egy kirakat alkalmazás lehet egy dokumentumot egy Termékváltozat, egy online számítógépes oktatási alkalmazás lehet egy dokumentumot egy tanfolyam, a kutatás vállalkozás előfordulhat, hogy van-e az egyes iskolai papír egy dokumentumot a tárházba, és így tovább.

Dokumentumok áll egy vagy több mezőt. Mezők tartalmazhat szöveget, amelyet az Azure Keresés parancs a keresési kifejezések tokenekre bontott, valamint a nem tokenekre bontott vagy nem szöveges értékeket a szűrők vagy pontozási profilok használható. A nevek, az adattípusok és az egyes mezők keresési funkciók a tárgymutató-séma határozza meg. Az egyes index sémában mezők valamelyikét egy ID ki kell jelölni, és minden a dokumentumhoz tartoznia kell egy értéknek az azonosító mezőt, amely egyedileg kell azonosítania a dokumentum a tárgymutató. Minden dokumentum mező nem kötelező, és fog alapértelmezés szerint a null érték, ha megadni. Figyelje meg, hogy null érték nem helyet foglalnak a keresési index.

Dokumentum feltöltéséhez előtt kell már korábban elkészült az index a szolgáltatás. [Create Index](#CreateIndex) talál részletes tudnivalókat az első lépést.

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Hozzáadása, frissítése, vagy dokumentumok törlése

Is feltölthet, egyesítése vagy a feltöltési és törlés egyesítése dokumentumok megadott indexből a HTTP-bejegyzés használatával. Frissítések nagyszámú kötegelés dokumentumok maximum (1000 dokumentumok egy köteg) vagy egy köteg körülbelül 16 MB ajánlott.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmet. Is feltölthet, megadott index létrehozása HTTP POST használata Körlevél egyesítése vagy a feltöltés vagy a törlés dokumentumait.

A kérés URI tartalmaz [index neve], mely index dokumentumait elküldheti megadása. Egyszerre csak egy index dokumentumok is közzé.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `Content-Type`: Kötelező. Beállítani`application/json`
- `api-key`: Kötelező. A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. A **Dokumentumok hozzáadása** kérelem tartalmaznia kell egy `api-key` fejlécben a rendszergazda kulcs (nem pedig a lekérdezés billentyűt).

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

A kérés törzsében indexelni kívánt egy vagy több dokumentumok tartalmazza. Dokumentumok egyedi kulcs azonosítja. Minden dokumentum társítva művelet: feltöltése, a körlevél, a mergeOrUpload vagy a törlés. Feltöltés kérésében szerepelnie kell a dokumentumadatok kulcs/érték párokká formájában.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Dokumentum kulcsok csak betűk, számok, a szaggatott vonal tartalmazhatnak ("-"), aláhúzásjelet ("_"), és az egyenlőségjelet ("="). További információra kíváncsi olvassa el a [névhasználati szabályok](https://msdn.microsoft.com/library/azure/dn857353.aspx)című témakört.

**Dokumentum-műveletek**

- `upload`: Feltöltés művelet hasonlít "upsert" Ha a dokumentum szúrja be a Ha az új, frissített/helyett akkor, ha van ilyen. Figyelje meg, hogy minden mező helyett jelennek meg a frissítés jelenik meg.
- `merge`: Egyesítés a megadott mezők egy meglévő dokumentumot frissíti. Ha a dokumentum nem létezik, az egyesítés sikertelen lesz. Minden mezőben adja meg, a körlevél lecseréli a meglévő mező a dokumentumban. Ide tartoznak a típusú mezők `Collection(Edm.String)`. Ha például a dokumentum tartalmaz "kódok" értékű mező `["budget"]` értéket az egyesítés végrehajtása és `["economy", "pool"]` "kódok", "kódok" mező a Végérték lesz `["economy", "pool"]`. Azt a program **nem** lehet `["budget", "economy", "pool"]`.
- `mergeOrUpload`: úgy viselkednek, mint `merge` , ha az index már létezik az adott billentyűt a dokumentumokat. Ha a dokumentum nem létezik úgy viselkednek, mint `upload` új dokumentumot.
- `delete`: Törlés az index eltávolítja a megadott dokumentumot. Figyelje meg, hogy minden olyan mezők, adja meg a `delete` figyelmen kívül hagyja a művelet nem kulcsmező. Ha el szeretné távolítani egy önálló mező a dokumentumban lévő, `merge` ehelyett és egyszerűen mező beállítása explicit módon való `null`.

**Válasz**

200 állapotkód (OK) értéket adja vissza az sikeres választ, ami azt jelenti, hogy minden elem sikeresen indexelt Ez jelzi a `status` tulajdonság beállítása igaz, az összes elem is látható, a `statusCode` tulajdonság 201 (az imént feltöltött dokumentumok) vagy a 200 (az egyesített cellákat vagy törölt dokumentumok):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Állapotkód 207 (többszörös állapot) ad vissza, ha nem tud indexelt legalább egy elemet. Nem indexelt cikkeknek a `status` mező értéke hamis. A `errorMessage` és `statusCode` tulajdonságok jelzi az indexelési hiba az az oka:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

A következő táblázat ismerteti a különféle dokumentum állapot kódokat vissza kell a válaszban. Figyelje meg, hogy problémát tapasztal a kérést, magát, néhány jelezheti, míg mások jelzi, hogy ideiglenes hiba feltételeket. Az utóbbi késleltetés után meg kell ismételni.

<table style="font-size:12">
    <tr>
        <th>Állapotkód</th>
        <th>Jelentése</th>
        <th>Újrapróbálkozást lehetővé tevő</th>
        <th>Jegyzetek</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokumentum sikeresen módosították vagy törölték.</td>
        <td>a #hiányzik</td>
        <td>Törlés-műveletek <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Ez azt jelenti, hogy akkor is, ha egy dokumentum kulcs nem létezik a tárgymutató, kísérel meg egy adott törlési művelet adott kulccsal eredményezi 200 állapotkódot.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokumentum sikeresen készült.</td>
        <td>a #hiányzik</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>A dokumentum indexelésből megakadályozó hiba történt.</td>
        <td>nem</td>
        <td>A hibaüzenet, a válasz jelenik meg, mi az a dokumentum mellett a megfelelő.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>A dokumentum nem lehet egyesíteni, mert az adott billentyűt az index nem létezik.</td>
        <td>nem</td>
        <td>Ez a hiba nem jelentkezik feltöltéshez, mivel ezek hozhat létre új dokumentumokat, és nem történik törli az mert túllépik <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Fájlverzió-ütközés észlelt, amikor megpróbál a tárgymutató-dokumentum.</td>
        <td>igen</td>
        <td>Ez akkor fordulhat elő, amikor egyidejűleg több, mint egyszer index ugyanabban a dokumentumban szeretné.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Az index átmenetileg nem érhető el, mert azt frissült a "allowIndexDowntime" jelzője "true" értékre van állítva.</td>
        <td>igen</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>A keresési szolgáltatás átmenetileg nem érhető el, esetleg nagy terhelés miatt.</td>
        <td>igen</td>
        <td>A kód előtt, hogy újra próbálkozik ebben az esetben kell várnia, vagy, kockázat szolgáltatás elérhetetlenné meghosszabbításáról.</td>
    </tr>
</table> 

**Megjegyzés**: Ha az ügyfél kód gyakran észlel 207 választ, több oka lehet, hogy a rendszer terhelés alatt. Győződhet meg, jelölje be a `statusCode` 503 tulajdonságait. Ha ez így, azt javasoljuk ***az indexelő kérések szabályozásának***. Ellenkező esetben forgalom az indexelés nem subside, ha a rendszer sikerült indítani elvetésével 503 a hibát tartalmazó összes kérelmet.

Állapotkód 429 azt jelzi, hogy a dokumentumok / index számú kvóta túllépte. Új index létrehozása vagy frissítése a magasabb kapacitás korlátozásairól.

**Példa:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Dokumentumok keresése

A **keresési** művelet kérése vagy KÜLDÉS kérésre kibocsátott, és adja meg a paraméterek, adjon meg a feltételnek megfelelő dokumentumok kijelölésére szolgáló.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Mikor érdemes használni a bejegyzés GET helyett**

HTTP GET hívja fel a **keresési** API használatakor tartsa szem előtt, hogy a kérelem URL-címe nem hosszabb 8 KB szeretne. Ez a legtöbb alkalmazással általában elég. Egyes alkalmazások azonban igen nagy lekérdezések vagy OData szűrőkifejezések termék. Ezeket az alkalmazásokat, a HTTP-bejegyzés mivel használata esetén célszerűbb lehetővé teszi a nagyobb, szűrők és lekérdezések GET-nál. A bejegyzés a kifejezéseket és a lekérdezés záradékok egy korlátozó tényező, nem a nyers lekérdezés, mivel a kérelem által bejegyzés körülbelül 16 MB méretű.

> [AZURE.NOTE] Annak ellenére, hogy a bejegyzés kérelem által igen nagy, a keresési lekérdezések és szűrőkifejezések tetszőlegesen összetett nem lehet. Lásd: [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) , és [Az OData-kifejezések szintaxisával](https://msdn.microsoft.com/library/dn798921.aspx) keresési lekérdezés és szűrés összetettsége korlátozások kapcsolatban további tudnivalókat.

**Kérés**

HTTPS szükség a szolgáltatási kérelmeket. A **keresési** kérelem kérése vagy KÜLDÉS módszerekkel kell kialakítani.

A kérés URI adja meg a lekérdezés az összes dokumentum, amelyek megegyeznek a paraméterek mely index. A GET-kérelmek esetében a lekérdezési karakterlánc megadott paramétereket, és a kérelem törzsébe, ha a bejegyzés kéri.

Legjobb módszer GET kérelmek létrehozásakor ne felejtse el egy adott lekérdezési paraméter [URL-kódolás](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) közvetlenül a REST API hívásakor. **Keresési** műveletekhez Ez tartalmazza:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-kódolás csak akkor javasolt a fenti lekérdezésparaméter. Ha véletlenül URL-kódolás, a teljes lekérdezési karakterlánc (minden után a?), kérések megszakad.

Ezenkívül URL-kódolás csak szükség a REST API segítségével közvetlenül a GET hívásakor. Nincs URL-kódolás szükség, hívja fel a **Keresés** használatával a bejegyzés vagy a [.NET-ügyfél tárban](https://msdn.microsoft.com/library/dn951165.aspx), amely kezeli az URL-kódolás meg használata esetén.

<a name="SearchQueryParameters"></a>
**A lekérdezés paraméterei**

**Keresés** fogadja el a több lekérdezési feltétel megadása és is megadhatja a keresési viselkedés paramétert. Megadhatja, hogy az URL-címet a paraméterek lekérdezési karakterlánc hívásakor **keresési** GET keresztül, valamint JSON tulajdonságok összehívás törzsében **keresési** bejegyzés keresztül hívásakor. Az egyes paraméterek szintaxisa némileg eltérő GET és POST között. Ezeket a különbségeket vannak időként vonatkozó alábbi:

`search=[string]`(nem kötelező) – a keresett szöveget. Az összes `searchable` mezőket alapértelmezés szerint vannak keresni, kivéve ha `searchFields` van megadva. Amikor keresés `searchable` mezők, a keresett szöveget magát tokenekre van bontott, így több feltétel üres terület kell elválasztani (például: `search=hello world`). Tetszőleges kifejezés megfeleltetéséhez használja `*` (Ez hasznos lehet logikai szűrő lekérdezések). A paraméter kimarad van eredménye ugyanaz, mint beállítása a következőre: `*`. A részletekért kattintson a keresési szintaxisa [Egyszerű lekérdezés szintaxisa](https://msdn.microsoft.com/library/dn798920.aspx) megtekintése

  - **Megjegyzés**: az eredmények esetenként is meglepő fölé lekérdezése esetén `searchable` mezőket. A tokenizáló angol nyelvű szöveg, például aposztrófot, számok, például vesszők közös esetek kezelésére logika tartalmazza. Ha például `search=123,456` fognak egyezni 123 és 456, egyéni kifejezések, hanem egy kifejezés 123,456 óta vessző ezer elválasztóként angolul nagy számok segítségével. Emiatt azt javasoljuk, írásjelek, hanem szóköz használata a feltételeket külön a `search` paraméter.

`searchMode=any|all`(nem kötelező, az alapértelmezett `any`) - e a keresési kifejezések közül választhat annak érdekében, hogy a dokumentum egyező megszámolása kell egymáshoz.

`searchFields=[string]`(nem kötelező) – a megadott szöveget a keresett mező vesszővel elválasztott nevek listáját. Célmezők jelöléssel kell `searchable`.

`queryType=simple|full`(nem kötelező, az alapértelmezett `simple`) – Ha "egyszerű" a keresett szöveg értékre van állítva, amely lehetővé teszi, hogy szimbólumok például egyszerű lekérdezés nyelvet használ értelmezett +, * és a "". Lekérdezések értékeli ki az összes kereshető mezőkben (vagy mezők szerint `searchFields`) alapértelmezés szerint minden dokumentumban. Ha a lekérdezés típusának beállítása `full` a Lucene lekérdezési nyelvről, amely lehetővé teszi, hogy a mező-specifikus és súlyozott keresések keresési szövegmezőjébe értelmezett. A részletekért kattintson a keresés szintaxis [Egyszerű lekérdezés szintaxisa](https://msdn.microsoft.com/library/dn798920.aspx) és a [Lekérdezés-szintaxis Lucene](https://msdn.microsoft.com/library/mt589323.aspx) megtekintése 
 
> [AZURE.NOTE] A lekérdezési nyelv helyett $filter hasonló funkciókat kínál, amelyek nem támogatott Lucene a keresési tartományt.

`moreLikeThis=[key]`(nem kötelező) **Fontos:** Ez a funkció csak a `2015-02-28-Preview`. Nem lehet használni ezt a beállítást, amely tartalmazza a szöveg keresési paraméteres lekérdezés `search=[string]`. A `moreLikeThis` paraméter talál, amelyek hasonlítanak a dokumentumot a dokumentum kulcs által megadott dokumentumokat. Ha a keresési kérelem történik `moreLikeThis`, keresési kifejezések listájának jön létre, a gyakoriság és ritkasága forrásdokumentumán kifejezések alapján. E kifejezések majd, hogy a kérést. Alapértelmezés szerint minden tartalmának `searchable` mezők számítanak, kivéve ha `searchFields` szeretné korlátozni a keresett mezőket használják.  

`$skip=#`(nem kötelező) – a keresési eredmények kihagyása; száma Nem lehet nagyobb, mint 100 000. Ha a dokumentumok beolvasása sorrendben kell, de nem használható `$skip` miatt ezt a korlátozást, akkor fontolja meg `$orderby` egy teljes mértékben rendelt kulcs és `$filter` tartományba lekérdezés helyette.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `skip` helyett `$skip`.

`$top=#`(nem kötelező) – a keresési eredmények beolvasásához számát. Ez is használható együtt `$skip` ügyféloldali lapozási a keresési eredmények végrehajtásához.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `top` helyett `$top`.

`$count=true|false`(nem kötelező, alapértelmezés szerint `false`) – meghatározza, hogy e-ból a találatok száma. Ez az összes dokumentumot tartalmazza, amelyek megegyeznek a számát a `search` és `$filter` paraméterek figyelmen kívül hagyása `$top` és `$skip`. Ez az érték beállítást `true` hatással lehet a teljesítmény. Ne feledje, hogy a számát adja vissza közelítő.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `count` helyett `$count`.

`$orderby=[string]`(nem kötelező) – az eredményeket, rendezése a kifejezések vesszővel tagolt listáját. Lehet, hogy minden kifejezés a mező nevét vagy a hívás a `geo.distance()` függvény. Minden egyes kifejezés követhetnek `asc` jelezni növekvő, és `desc` , hogy a csökkenő sorrendben. Az alapértelmezett van növekvő sorrendben. TIES oszlanak a dokumentumok az egyező értékek szerint. Ha nincs `$orderby` van megadva, az alapértelmezett rendezési sorrend van csökkenő dokumentum egyezés pontszám szerint. A 32 záradékok korlátozott `$orderby`.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `orderby` helyett `$orderby`.

`$select=[string]`(nem kötelező) - beolvasásához vesszővel tagolt mezők listája. Ha nincs megadva, az összes mezők szerint beolvasható a sémában megjelölt szerepelnek. A paraméter beállításával közvetlenül is kérhet minden mező `*`.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `select` helyett `$select`.

`facet=[string]`(nulla vagy több) – egy mezőt, amelyet dimenzió. Tetszés szerint a karakterlánc is tartalmazhatnak, ha testre szeretné szabni a vesszővel elválasztott kifejezett faceting paraméterek `name:value` párokká. Érvényes paraméterek a következők:

- `count`(maximális száma dimenzió feltételek; az alapértelmezett érték 10). Nincs korlátozás van, de a magasabb értékek megfelelő teljesítményét merülnek fel, különösen akkor, ha a síklapos mező tartalmazza a sok egyedi feltételeket.
  - Példa: `facet=category,count:5` kapja a legjobb öt kategóriák dimenzió között.  
  - **Megjegyzés**: Ha a `count` paraméter értéke kisebb, mint az egyedi feltételeket, előfordulhat, hogy az eredmények nem pontosak. Ez a faceting lekérdezések vannak elosztva shards módja miatt. Növekvő `count` általában az a szerződési időszak száma, de a teljesítmény költségére pontosabb lesz.
- `sort`(egyik `count` a darabszám szerint *csökkenő sorrendbe* rendezheti `-count` a darabszám szerint *növekvő* rendezése `value` *növekvő* , érték szerinti rendezése vagy `-value` *Csökkenő* érték szerinti rendezéshez)
  - Példa: `facet=category,count:3,sort:count` megkapja a felső három kategóriába dimenzió eredmények város nevét dokumentumok száma szerint csökkenő sorrendben. Ha például a felső három kategóriába: a tervezett, amelyben és engedélyezhető, és a tervezett 5 találatok van, amelyben 6 van, és engedélyezhető 4, akkor az időszakok lesz abban a sorrendben amelyben, a tervezett, engedélyezhető.
  - Példa: `facet=rating,sort:-value` az összes lehetséges minősítések érték szerint csökkenő sorrendbe időszakok eredményez. Például ha a minősítések 1-től 5-ig, az időszakok fog rendelhető 5, 4-es, 3, 2, 1, függetlenül attól, hány dokumentumok felel meg az egyes minősítés.
- `values`(a numerikus függőleges vonás tagolt vagy `Edm.DateTimeOffset` értékeket tartalmazó dimenzió bejegyzés dinamikus értékhalmaz)
  - Példa: `facet=baseRate,values:10|20` három időszakok eredménye: egy alap ráta 0, de nem beleértve legfeljebb 10, és egy 10, de nem beleértve 20 és egy 20 vagy újabb verziójában.
  - Példa: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` hoz létre két időszakok: egy szállodák felújított előtt február 2010 és az szállodák felújított február 1st, 2010-es vagy újabb verziója.
- `interval`(nagyobb, mint 0, számok, egész időköz vagy `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` dátum-idő érték)
  - Példa: `facet=baseRate,interval:100` időszakok mérete 100 alap ráta cellatartományok alapján készít. Például az összes közötti $60 és $600 alap díjak esetén lesz a 0-100, 100-200, 200-300, 300-400, 400-500 és 500-600 időszakok.
  - Példa: `facet=lastRenovationDate,interval:year` hoz létre egy gyűjtő az egyes mikor szállodák voltak felújított.
- `timeoffset`([+-] hh: mm, [+-] hhmm, vagy [+-] hh) `timeoffset` nem kötelező. Azt is csak lehet kombinálni a `interval` lehetőséget, és csak akkor, ha egyenként alkalmazza őket típusú mezőhöz `Edm.DateTimeOffset`. Az érték eltolás UTC idő-fiókjához idő határai beállítása.
  - Példa: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` használ, amely elindítja a nap határérték: 01:00:00 UTC (cél időzónát a éjfél)
- **Megjegyzés**: `count` és `sort` az azonos dimenzió beállítások egyesíthetők, de azok nem lehet kombinálni `interval` vagy `values`, és `interval` és `values` együtt nem használható együtt.
- **Megjegyzés**: a dátum-idő intervallum metszettel vannak számított Egyezményes világidő alapján, ha `timeoffset` nincs megadva. Példa: a `facet=lastRenovationDate,interval:day`, a nap oszlopazonosító 00:00:00 UTC kezdődik. 

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `facets` helyett `facet`. Is akkor adja meg mindegyik szöveges karakterláncot egy külön dimenzió kifejezés esetén karakterláncok JSON tömbként.

`$filter=[string]`(nem kötelező) – A szabványos OData-szintaxisú strukturált keresőkifejezést. Lásd: [OData kifejezések szintaxisával](#ODataExpressionSyntax) kapcsolatos részleteket, amely támogatja az Azure keresési OData kifejezés nyelvhelyesség részét.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `filter` helyett `$filter`.

`highlight=[string]`(nem kötelező) – egy vesszővel tagolt mezőnevek halmazából találat használt kiemeli. Csak `searchable` mezők a találatok kiemelése használhatók.

`highlightPreTag=[string]`(nem kötelező, alapértelmezés szerint `<em>`) – olyan karakterlánc, amely a találati kiemelések lefoglalja címke. Be kell az `highlightPostTag`.

> [AZURE.NOTE] Amikor hívása **keresési** GET, az URL-cím foglalt karakterek használata kell százalék-kódolni (például # helyett: 23 %).

`highlightPostTag=[string]`(nem kötelező, az alapértelmezett `</em>`)-karakterlánc, amely hozzáfűzi a találati kiemelések címke. Be kell az `highlightPreTag`.

> [AZURE.NOTE] Amikor hívása **keresési** GET, az URL-cím foglalt karakterek használata kell százalék-kódolni (például # helyett: 23 %).

`scoringProfile=[string]`(nem kötelező) - ki szeretné számítani pontozási profil nevét megfelelő értékek dokumentumok megfelelő találatok rendezése érdekében.

`scoringParameter=[string]`(nulla vagy több) - jelzi az egyes paraméterek pontozási függvény a megadott értékeket (például `referencePointParameter`) formátumú `name-value1,value2,...`.

- Ha például a pontozási profil függvény "mylocation" nevű paraméterrel határozza meg, a lekérdezési karakterlánc lehetőséget akkor `&scoringParameter=mylocation--122.2,44.8`. Az első kötőjel elválasztja a nevét az értéklista közben a második kötőjel az első érték (ebben a példában hosszúság) része.
- Pontozási paraméterekkel címke szolgáló lehetőségekről, amely tartalmazhat vesszőkkel, mint bármely ezeket az értékeket a listában, használja a aposztrófok escape. Saját maguk értékeket tartalmaznak aposztrófok, is escape Duplázással.
  - Például ha szolgáló lehetőségekről paraméter neve "mytag" címke van, és kattintson a címke növelése szeretne értékek "Hello O'Brien" és "Kovács", a lekérdezési karakterlánc beállítás lenne `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Ne feledje, hogy idézőjelek csak vesszőt tartalmazó értékek szükséges.

> [AZURE.NOTE] Ez a paraméter neve **Keresés** használatával a bejegyzés hívásakor `scoringParameters` helyett `scoringParameter`. Ezenkívül Ön adja meg mindegyik szöveges karakterláncot esetén külön karakterláncok JSON tömbként `name-values` pár.

`minimumCoverage`(nem kötelező, az alapértelmezett 100)-0 és 100 jelző, amely kell hatálya alá ahhoz, hogy a lekérdezés kell jelenteni, a siker keresési lekérdezés az index százalékos közé eső szám. Alapértelmezés szerint a teljes indexben kell rendelkezésre állnia vagy `Search` HTTP állapotkód 503 ad eredményül. Ha a `minimumCoverage` és `Search` létrejött, HTTP 200 vissza, és olyan egy `@search.coverage` a választ, jelezve az indexet, a lekérdezés szereplő százalékos értékét.

> [AZURE.NOTE] A paraméter értéke 100-nál kisebb akkor lehet hasznos, annak biztosítására, akár csak egy replikával szolgáltatások keresési elérhetőségét. Azonban nem az összes megfelelő dokumentumok vannak garantált a keresési eredmények között szerepel. Ha keresési visszahívási fontosabb az alkalmazás, mint az elérhetőség, akkor célszerű hagyja `minimumCoverage` a 100 az alapértelmezett érték.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

Megjegyzés: az ezt a műveletet a `api-version` az URL-cím, függetlenül attól, hogy hívja fel az **keresési** kérése vagy KÜLDÉS lekérdezési paraméter van megadva.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterláncot, a szolgáltatás URL-címre egyedi. Megadhatja, hogy a **keresési** kérelem egy rendszergazda billentyűt vagy a lekérdezés kulcsa `api-key`.

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

GET-: nincs.

Bejegyzés:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Keresés részleges válaszok fenntartása**

Előfordul, hogy Azure keresés nem adja vissza az összes kért eredmény az egy keresési választ. Ez akkor fordulhat elő, például amikor a lekérdezés kérelmeket nem megadásával túl sok dokumentum más okokból `$top` vagy értéket ad `$top` túl nagy. Ezekben az esetekben az Azure keresési tartalmazni fogja a `@odata.nextLink` széljegyzet válasz törzsébe, továbbá `@search.nextPageParameters` Ha POST kérelem volt. Lépjen a következő rész a keresési választ egy másik keresési kérelem megfogalmazásához használhatja ezeket a jegyzeteket az értékeket. Az eredeti keresési kérelem ***folytatását jelző*** Link, és a széljegyzeteket általában úgynevezett ***folytatását jelző tokenek***. Lásd [az alábbi példában](#SearchResponse) a részletes tudnivalókat az alábbi ezeket a jegyzeteket, és hol helyezkednek el a válasz törzsébe. 

Miért Azure keresési feltétlenül járnak a folytatását jelző tokenek okai végrehajtása-specifikus és módosítása. Robusztus ügyfelek mindig készen áll a kezelheti az esetekben, amikor a visszaadott vártnál kevesebb dokumentumot, és egy folytatását jelző token megtalálható folytatni a dokumentumok kell lennie. Tartsa szem előtt kell használnia HTTP ugyanazzal a módszerrel a eredeti kérelem annak érdekében, hogy továbbra is. Például ha GET kérésekben küldte, minden folytatását jelző kérelem küld is használni kell GET (és hasonlóképpen BEJEGYZÉSBE).

<a name="SearchResponse"></a>
**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Példa:**

Az [OData-kifejezések szintaxisával Azure keresés](https://msdn.microsoft.com/library/azure/dn798921.aspx) lapon talál további példákat.

1)  Keresés az Index a dátum szerint csökkenő sorrendben rendezve.


    GET-/indexes/hotels/docs? keresési = * & $orderby = lastRenovationDate desc & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "*", "orderby": "lastRenovationDate desc"}

2)  Síklapos keresés keresse meg az indexet, és metszettel kategóriák, a minősítés, a címkék, valamint az adott tartományok baseRate elemek számára beolvasása:


    GET-/indexes/hotels/docs? keresési = próba & dimenzió = kategória & dimenzió = minősítés & dimenzió címkék és dimenzió = baseRate, értékek: 80 = |} 150 |} 220 és az api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "tesztelése", "metszettel": ["kategória", "minősítés", "kódok", "baseRate, értékek: 80 |} 150 |} 220"]}

3)  Az előző síklapos lekérdezés eredménye egy szűrővel szűkítéséhez, után a felhasználó gombra kattint, a 3-as és a kategória "Amelyben" minősítés:


    GET-/indexes/hotels/docs? keresési = próba & dimenzió címkék és dimenzió = baseRate, értékek: 80 = |} 150 |} 220 & $filter minősítés 3, és amelyben"kategória eq" a kö & api-verzió = = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "tesztelése", "metszettel": ["kódok", "baseRate, értékek: 80 |} 150 |} 220"], "szűrő": "a minősítési eq 3-as és a kategória eq"Amelyben""}

4) Állítsa a hibafüggvény síklapos keresést, a lekérdezés által visszaadott egyedi feltétel szerint. Az alapértelmezett érték 10, de növelheti vagy csökkentheti a érték használata a `count` a paramétert a `facet` attribútum:


    GET-/indexes/hotels/docs? keresési = próba & dimenzió = város, száma: 5 & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "tesztelése", "metszettel": ["város, száma: 5"]}

5)  Keresse meg a belül az adott mezők; Ha például egy nyelvspecifikus mezőt:


    GET-/indexes/hotels/docs? keresési = hôtel & searchFields = description_fr & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "hôtel", "searchFields": "description_fr"}

6) Keresse meg a több mezőkben. Például tárolására és a lekérdezés kereshető mezőket az összes azonos indexszel belül több nyelven.  Ha angol vagy francia leírások másokkal közösen ugyanazon a dokumentumon belül létezik, visszaállíthatja a lekérdezés eredményében végezheti el:


    GET-/indexes/hotels/docs? keresési = szállások & searchFields = leírás, description_fr & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "szállások", "searchFields": "leírás, description_fr"}

Figyelje meg, hogy is csak lekérdezhetők indexet egyszerre. Ne hozzon létre több indexeket az egyes nyelvekhez, kivéve, ha azt tervezi, hogy a lekérdezés egyesével.

7)  Lapozás - kérése az elemek 1st oldalára (oldalméret a 10):


    GET-/indexes/hotels/docs? keresési = * & $skip = 0 & $top = 10 & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "*", "átugrása": 0 "felső": 10}

8)  Lapozás - kérése az elemek 2nd oldalára (oldalméret a 10):


    GET-/indexes/hotels/docs? keresési = * & $skip = 10 & $top = 10 & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "*", "átugrása": 10, a "felső": 10}

9)  A mezők meghatározott beolvasása:


    GET-/indexes/hotels/docs? keresési = * & $select hotelName, leírás és az api-verzió = = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "*", "select": "hotelName leírás"}

10)  Egy adott szűrőkifejezés megfelelő dokumentumok beolvasása:


    GET-/indexes/hotels/docs? $filter = (baseRate ge 60 és baseRate lt 300) vagy hotelName tetszetős kapcsolatának egyenlő a kö & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"szűrő": "(baseRate ge 60 és baseRate lt 300) vagy hotelName eq"Tetszetős megőrizheti""}

11) Keresés az index és a visszaküldési töredékek a találati legfontosabb elemei


    GET-/indexes/hotels/docs? keresési = egy üzenetet, és jelölje ki =, leírás és az api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "valami", "kiemelése": "leírás"}

12) Keresse meg a és a dokumentumok hivatkozást távolabb megtalálhatók közelebb helyről rendezett visszatérési


    GET-/indexes/hotels/docs? keresési valamit, amit = & $orderby=geo.distance (helyét, geography'POINT(-122.12315 47.88121) ") & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "valami", "orderby": "geo.distance (helyét, geography'POINT(-122.12315 47.88121)") "}

13) Keresse meg a feltételezve, hogy "geo" nevű pontozási profil függvényekkel két távolság pontozási, "currentLocation" és "lastLocation" nevű paraméter megadása egy egy definiáló paraméter neve


    GET-/indexes/hotels/docs? keresési valamit, amit = & scoringProfile = geo & scoringParameter currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 és az api-verzió = = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "valami", "scoringProfile": "geo", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}

14) Dokumentumok keresése a tárgymutató [Egyszerű lekérdezés-szintaxis](https://msdn.microsoft.com/library/dn798920.aspx)használatára. A lekérdezés Ha kereshető mezőket tartalmaz, a kifejezéseket "irodai" és "hely", de nem "amelyben" Szállodák adja eredményül:


    GET-/indexes/hotels/docs? keresési = irodai + hely – amelyben & searchMode = all & api-verzió = 2015-02-28 – előzetes verzió

    BEJEGYZÉS /indexes/hotels/docs/search? api-verzió 2015-02-28 – előzetes verzió = {"Keresés": "irodai + hely – amelyben", "searchMode": "az összes"}

Megjegyzés: az e `searchMode=all` felett. Többek között a paraméter felülbírálja az alapértelmezett `searchMode=any`biztosítja, hogy `-motel` azt jelenti, hogy ", és nem" helyett "Vagy NOT". Nélkül `searchMode=all`, "Vagy nem", amely kibővíti korlátozza a keresési eredmények helyett kap, és ez lehet az egyes felhasználók counter-intuitive.

15) Dokumentumok keresése a tárgymutató [lucene lekérdezési szintaxis](https://msdn.microsoft.com/library/mt589323.aspx)használatára. Ezt a lekérdezést szállodák adja eredményül, amelyek a kategória mezőben szerepel az "tervezett" kifejezés és a "nemrégiben felújított" kifejezést tartalmazó összes kereshető mező. A "nemrégiben felújított" kifejezést tartalmazó dokumentumok magasabb rangsorolásának eredményeként a szerződési időszak erősítése értéket (3)

    GET-/indexes/hotels/docs?search kategória: tervezett = és \"nemrégiben felújított\"^ 3 & searchMode = all & api-verzió 2015-02-28 – előzetes verzió = & querytype = teljes

    BEJEGYZÉS /indexes/hotels/docs/search?api-version 2015-02-28 – előzetes verzió = {"Keresés": "kategória: tervezett és \"nemrégiben felújított\"^ 3", "queryType": "teljes", "searchMode": "az összes"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>A keresés dokumentum

A **Keresési dokumentum** művelet dokumentum átveszi Azure keresés. Ez akkor hasznos, amikor a felhasználó egyedi keresési eredmény kattint, és a lépések elvégzésével keresheti meg, hogy a dokumentum pontos részleteket szeretne.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Kérés**

HTTPS szükség a szolgáltatási kérelmeket. A **Dokumentum keresési** kérelem módon is alakítható ki.

    GET /indexes/[index name]/docs/key?[query parameters]

Azt is megteheti fő keresési a hagyományos OData-szintaxis is használható:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

A kérés URI tartalmaz egy [index neve] és [kulcs], mely a dokumentum mely indexből beolvasásához megadása. Egyszerre csak egy dokumentum elérheti. Több dokumentum lépéseivel a egyetlen kérelem használja a **keresőt** .

**A lekérdezés paraméterei**

`$select=[string]`(nem kötelező) - beolvasásához vesszővel tagolt mezők listája. Ha nincs megadva, vagy állítsa `*`, az a kivetítés szereplő összes mező szerint beolvasható a sémában megjelölt.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

Megjegyzés: az ezt a műveletet a `api-version` lekérdezési paraméter van megadva.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterláncot, a szolgáltatás URL-címre egyedi. Megadhatja, hogy a **Dokumentum keresési** kérelem egy rendszergazda billentyűt vagy a lekérdezés kulcsa `api-key`.

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

Nincs lehetőség.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Példa**

Keresés a dokumentumot, amelynek kulcsa "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Keresés a dokumentumot, amelynek az OData-szintaxissal 3-as kulcs:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Dokumentumok száma

A **Dokumentumok száma** művelet olvassa be a keresési index lévő dokumentumok száma. A `$count` szintaxis az OData-protokoll része.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Kérés**

HTTPS szükség a szolgáltatási kérelmeket. A **Dokumentumok száma** kérelem is alakítható ki a GET metódussal.

A kérelem URI [index neve] azt jelenti, hogy a szolgáltatást, hogy a megadott index dokumentumok gyűjteménye az összes elem számát adja vissza.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében.

- `Accept`: Meg kell ez az érték `text/plain`.
- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterláncot, a szolgáltatás URL-címre egyedi. A **Dokumentumok száma** kérelem adhatja meg egy rendszergazda billentyűt vagy a lekérdezés kulcsa `api-key`.

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

Nincs lehetőség.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

A válasz szervezet egy egyszerű szöveg formátumú egész darab értéket tartalmazza.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Javaslatok

A **javasolt** művelet olvassa be a javaslatok alapján részleges keresést beviteli. A szokásos használatban van keresőmezők felhasználók írnak a keresési feltételek szerint automatikusan felajánlott javaslatok megadását.

Javaslat kérések célja: illusztráló cél a dokumentumok, így a javasolt szöveg megismételhető, ha több jelölt dokumentum felel meg a bemeneti ugyanazon keresés. Használható `$select` , hogy melyik dokumentum a forrás minden javaslathoz kiderítheti lekérdezni más dokumentumok mezőket (beleértve a dokumentum billentyűt).

A **javasolt** művelet kibocsátott kérése vagy KÜLDÉS kérelmet.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Mikor érdemes használni a bejegyzés GET helyett**

HTTP GET hívja fel a **javaslatok** API használatakor tartsa szem előtt, hogy a kérelem URL-címe nem hosszabb 8 KB szeretne. Ez a legtöbb alkalmazással általában elég. Egyes alkalmazások azonban igen nagy lekérdezések kifejezetten OData szűrőkifejezések hozhat létre. Ezeket az alkalmazásokat, a HTTP-bejegyzés mivel használata esetén célszerűbb lehetővé teszi a GET-nál nagyobb szűrők. A bejegyzés a szűrő záradék egy korlátozó tényező, nem a nyers szűrő karakterláncot, mivel a kérelem által bejegyzés körülbelül 16 MB méretét.

> [AZURE.NOTE] Annak ellenére, hogy a bejegyzés kérelem által igen nagy, szűrőkifejezések tetszőlegesen összetett nem lehet. Lásd: [Az OData-kifejezések szintaxisával](https://msdn.microsoft.com/library/dn798921.aspx) szűrő összetettsége korlátozások kapcsolatban további tudnivalókat.

**Kérés**

HTTPS szükség a szolgáltatási kérelmeket. A **javaslatok** kérelem kérése vagy KÜLDÉS módszerekkel kell kialakítani.

A kérés URI megadja a nevét, az index lekérdezés. A GET-kérelmek esetében a lekérdezési karakterlánc megadott paramétereket részben beviteli keresőkifejezést, például, és a kérelem törzsébe, ha a bejegyzés kéri.

Legjobb módszer GET kérelmek létrehozásakor ne felejtse el egy adott lekérdezési paraméter [URL-kódolás](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) közvetlenül a REST API hívásakor. **Javaslatok** műveletekhez Ez tartalmazza:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-kódolás csak akkor javasolt a fenti lekérdezésparaméter. Ha véletlenül URL-kódolás, a teljes lekérdezési karakterlánc (minden után a?), kérések megszakad.

Ezenkívül URL-kódolás csak szükség a REST API segítségével közvetlenül a GET hívásakor. Nincs URL-kódolás szükség, hívja fel a **javaslatok** használata bejegyzés vagy a [.NET-ügyfél tárban](https://msdn.microsoft.com/library/dn951165.aspx), amely kezeli az URL-kódolás meg használata esetén.

**A lekérdezés paraméterei**

**Javaslatok** fogadja el a szükséges lekérdezési feltétel és is megadhatja a keresési viselkedés több paramétert. Megadhatja, hogy az URL-címet a paraméterek lekérdezési karakterlánc hívásakor **javaslatok** GET keresztül, valamint JSON tulajdonságok összehívás törzsében **javasolt** bejegyzés keresztül hívásakor. Az egyes paraméterek szintaxisa némileg eltérő GET és POST között. Ezeket a különbségeket vannak időként vonatkozó alábbi:

`search=[string]`-a keresett szöveget javaslat lekérdezéseket használni. Legalább 1 karakter és 100-nál több karakterből kell lennie.

`highlightPreTag=[string]`(nem kötelező) - karakterlánc címkézése, hogy a keresési találatok lefoglalja. Be kell az `highlightPostTag`.

> [AZURE.NOTE] Amikor hívása **javaslatok** GET, az URL-cím foglalt karakterek használata kell százalék-kódolni (például # helyett: 23 %).

`highlightPostTag=[string]`(nem kötelező) - karakterlánc címkézéséhez, amely hozzáfűzi a keresési találatok. Be kell az `highlightPreTag`.

> [AZURE.NOTE] Amikor hívása **javaslatok** GET, az URL-cím foglalt karakterek használata kell százalék-kódolni (például # helyett: 23 %).

`suggesterName=[string]`-a meghatározott suggester nevét a `suggesters` , amelyekben az index definíciója része. A `suggester` határozza meg, hogy mely mezők a feltételek a lekérdezéshez felkínált rendszer víruskeresést hajtson végre. [Suggesters](#Suggesters) talál további információt.

`fuzzy=[boolean]`(nem kötelező, alapértelmezett = false) – Ha ez az API TRUE kifejezésre keresve javaslatok még akkor is, ha a keresett szöveget helyettesített vagy hiányzó karakter szerepel. Amíg ez bizonyos helyzetekben jobb felületet nyújt megtalálható a költség zavaros javaslatok keresések lassabban, és további források felhasználása teljesítményét.

`searchFields=[string]`(nem kötelező) – a megadott keresési szöveget a keresett mező vesszővel elválasztott nevek listáját. Célmezők vonatkozó ajánlások engedélyezve kell lennie.

`$top=#`(nem kötelező, alapértelmezett = 5) – a javaslatok beolvasásához számát. 1 és 100 közötti számnak kell lennie.

> [AZURE.NOTE] A paraméter neve **javasolt** bejegyzés használatával hívásakor `top` helyett `$top`.

`$filter=[string]`(nem kötelező) - javaslatok egy kifejezés, amely a dokumentumok szűrők figyelembe venni.

> [AZURE.NOTE] A paraméter neve **javasolt** bejegyzés használatával hívásakor `filter` helyett `$filter`.

`$orderby=[string]`(nem kötelező) – az eredményeket, rendezése a kifejezések vesszővel tagolt listáját. Lehet, hogy minden kifejezés a mező nevét vagy a hívás a `geo.distance()` függvény. Minden egyes kifejezés követhetnek `asc` jelezni növekvő, és `desc` , hogy a csökkenő sorrendben. Az alapértelmezett van növekvő sorrendben. A 32 záradékok korlátozott `$orderby`.

> [AZURE.NOTE] A paraméter neve **javasolt** bejegyzés használatával hívásakor `orderby` helyett `$orderby`.

`$select=[string]`(nem kötelező) - beolvasásához vesszővel tagolt mezők listája. Ha nincs megadva, csak a dokumentum kulcs és javaslatok szöveget adja vissza. Explicit módon kérheti az összes mező értékre állításával a paraméter `*`.

> [AZURE.NOTE] A paraméter neve **javasolt** bejegyzés használatával hívásakor `select` helyett `$select`.

`minimumCoverage`(nem kötelező, az alapértelmezett 80)-0 és 100, az index, amely ahhoz, hogy a lekérdezés kell jelenteni, a siker javaslatok lekérdezés bemutatásához százalékos jelző közé eső szám. Alapértelmezés szerint az index legalább 80 %-át kell érhető el vagy `Suggest` HTTP állapotkód 503 ad eredményül. Ha a `minimumCoverage` és `Suggest` létrejött, HTTP 200 vissza, és olyan egy `@search.coverage` a választ, jelezve az indexet, a lekérdezés szereplő százalékos értékét.

> [AZURE.NOTE] A paraméter értéke 100-nál kisebb akkor lehet hasznos, annak biztosítására, akár csak egy replikával szolgáltatások keresési elérhetőségét. Azonban nem az összes egyező javaslatok vannak garantált bemutató a találatok között. Ha visszahívási fontosabb az alkalmazás, mint az elérhetőség, akkor célszerű nem alsó `minimumCoverage` az alapértelmezett érték 80 alatt.

`api-version=[string]`(kötelező). Az előzetes verzió `api-version=2015-02-28-Preview`. Lásd: [Keresési szolgáltatás verziószámozás](http://msdn.microsoft.com/library/azure/dn864560.aspx) információ és más verziói.

Megjegyzés: az ezt a műveletet a `api-version` az URL-cím, függetlenül attól, hogy hívja fel az **javaslatok** kérése vagy KÜLDÉS lekérdezési paraméter van megadva.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécek

- `api-key`: A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterláncot, a szolgáltatás URL-címre egyedi. A **javaslatok** kérelem adhat meg, akár egy rendszergazdai vagy lekérdezés billentyűt, a `api-key`.

A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az Azure-portálra a szolgáltatás irányítópultjáról. Lásd: [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) az Oldalválasztás segítségével.

**Szervezet kérése**

GET-: nincs.

Bejegyzés:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Ha a kivetítés beállítás mezők beolvasásához használt szerepelnek a tömb összes elemét:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Példa**

Hol a keresés részleges bemeneti a "lux" 5 javaslatok beolvasása

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
