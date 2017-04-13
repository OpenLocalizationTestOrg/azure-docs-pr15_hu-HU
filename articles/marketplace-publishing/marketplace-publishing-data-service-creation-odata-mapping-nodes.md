<properties
   pageTitle="Útmutató a adatok szolgáltatás létrehozása a piactér |} Microsoft Azure"
   description="Részletes útmutatást, hogy miként hozhat létre, tanúsítása és az adatok szolgáltatás üzembe vásárolja meg a Microsoft Azure piactéren."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Egy meglévő webszolgáltatás leképezés OData CSDL keresztül a csomópontok séma ismertetése

>[AZURE.IMPORTANT] **Adott időben azt már nem bevezetési minden új adatszolgáltatás közzétevők. Új dataservices nem első jóváhagyva listaelem.** Ha a szoftver üzleti alkalmazások AppSource a közzétenni kívánt talál további információt [Itt](https://appsource.microsoft.com/partners). Ha egy IaaS alkalmazások vagy Fejlesztőeszközök szolgáltatás szeretné közzé a Microsoft Azure piactéren, hogy több információt találhat [az alábbi](https://azure.microsoft.com/marketplace/programs/certified/).

A dokumentum segítséget nyújt a csomópont diagramszerkezetek áttekinthetővé egy OData protokollhoz hozzárendelése CSDL. Fontos tudni, hogy jól-e a csomópont-struktúra formázott XML. Így legfelső szintű, szülő-gyermek séma az OData-megfeleltetés tervezésekor kell alkalmazni.

## <a name="ignored-elements"></a>Figyelmen kívül hagyott elemek
Az alábbiakban a magas szintű CSDL elemeket (XML csomópontok), amelyek nem fog a webszolgáltatás metaadatok az importálás során a Microsoft Azure piactéren kódmentes való használatra. Azok a bemutató lehet, de figyelmen kívül hagyja.

| Elem | Hatókör |
|----|----|
| Elem használatával | A csomópontot, a sub csomópontok és az összes attribútumok |
| Dokumentáció elem | A csomópontot, a sub csomópontok és az összes attribútumok |
| ComplexType | A csomópontot, a sub csomópontok és az összes attribútumok |
| A társítási elem | A csomópontot, a sub csomópontok és az összes attribútumok |
| Kiterjesztett tulajdonság | A csomópontot, a sub csomópontok és az összes attribútumok |
| EntityContainer | A következő attribútumok a program figyelmen kívül hagyja: *fejlettebb* és *AssociationSet* |
| Séma | A következő attribútumok a program figyelmen kívül hagyja: *Namespace* |
| FunctionImport | A következő attribútumok a program figyelmen kívül hagyja: *mód* (az ln alapértelmezett értéket veszi alapul) |
| EntityType | A következő sub csomópontok a program figyelmen kívül hagyja: *kulcs* és *PropertyRef* |

A következő ismerteti a módosítások (hozzáadott és figyelmen kívül hagyja az elemeket) a különböző CSDL XML csomópontjainak részletesen.

## <a name="functionimport-node"></a>FunctionImport csomópontot.
Egy FunctionImport csomópont jelöl egy URL-cím (belépési pontjához) elérhetővé teszi a végfelhasználói a szolgáltatást. A csomópont lehetővé teszi, hogy hogyan az URL-cím meg van címezve leíró, mely paramétere érhető el, hogy a végfelhasználói, hogy ezek a paraméterek állnak rendelkezésre.

A csomópont részleteket talál a [itt], [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Az alábbiakban további attribútumok (vagy attribútumokat kiegészítése), amely a FunctionImport csomópont által elérhetővé tett:

**d:BaseUri** -
a URI-sablon a többi erőforrás van kitéve piactérről. A piactér a sablon a REST web service lekérdezéseket összeállításához. URI sablon tartalmaz, a {parameterName}, formájában paraméterekkel helyőrzők parameterName esetén a paraméter neve. Számbavétele apiVersion = {apiVersion}.
Paraméterek engedélyezett URI paraméterként vagy URI elérési részeként jelennek meg. Az elérési út megjelenése esetén azok mindig kötelező (nem jelölhető szerint nullable). *Példa:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Név** - az importált függvény neve.  Nem lehet ugyanaz, mint a CSDL más definiált nevek.  Számbavétele Név = "GetModelUsageFile"

**EntitySet** *(nem kötelező)* – a függvény visszatérési értéke a személy típusú gyűjtemény a **EntitySet** értékének kell lennie a szervezet beállítása, amelyhez tartozik a gyűjteményben. Egyéb esetben a **EntitySet** attribútum nem használható. *Példa:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(Nem kötelező)* - az adja meg a URI által visszaadott elemek.  Ne használja az attribútum, ha a függvény nem ad vissza értéket. Támogatott fájltípusok a következők:

 - **Webhelycsoport (<Entity type name>)**: Itt adhatja meg a meghatározott személy típusú gyűjteménye. A név szerepel a név attribútum EntityType csomópont. Példa az webhelycsoport (WXC. HourlyResult).
 - **Nyers (<mime type>)**: Itt adhatja meg a nyers dokumentum/blob, amely a felhasználó kapja. Példa az Raw(image/jpeg) további példákat:

  - ReturnType="Raw(text/plain)"
  - ReturnType = "webhelycsoport (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** – Itt adhatja meg, hogyan lapozási kezeli a többi erőforrás. A paraméter értékeket fogja használni belül kapcsos ágazatok, például lap {$page} = & az elemek laponkénti száma = {$size} elérhető beállítások a következők:

- **Nincs:** nincs lapozási nem érhető el
- **Kihagyása:** lapozási fejezi ki egy logikai "Ugrás" és "take" (felső) keresztül. Kihagyása metszéspontok M elemek fölé, és take adja vissza a következő N elemeket. Paraméter: $skip
- **Vennie:** Jegyzetelés a következő N elemeket adja eredményül. Paraméter: $take
- **PageSize értékének:** lapozási fejezi ki egy logikai lap és a méret (oldalanként cikkek). Lap jelöli az aktuális lapról, a függvény által visszaadott. Paraméter: $page
- **Méret:** méret minden lap esetén a visszaadott elemek számát jelenti. Paraméter: $size

**d:AllowedHttpMethods** *(Nem kötelező)* – Itt adhatja meg, mely igei kezeli a többi erőforrás. Elfogadott igei is korlátozza a megadott értéket.  Alapértelmezett = BEJEGYZÉST.  *Példa:* `d:AllowedHttpMethods="GET"` Elérhető beállítások a következők:

- **Első:** általában használt adatok
- **Bejegyzés:** általában használt új adatok beszúrása
- **Helyezése:** általában használt adatok frissítése
- **Törlése:** adatok törlése

További gyermekcsomópontjainak (nem vonatkozik a CSDL dokumentációt) belül a FunctionImport csomópontot a következők:

**d:RequestBody** *(Nem kötelező)* - összehívás törzsébe jelzi, hogy a kérelem vár küldeni egy szervezet szolgál. Paraméterek kaphatnak a összehívás törzsében. Vannak megadva kapcsos zárójelek, például {parameterName}. Ezeket a paramétereket a az átkerül a tartalomszolgáltató szolgáltatás törzsébe a felhasználó által vannak megfeleltetve. Az requestBody elemet a név httpMethod attribútummal. Az attribútum lehetővé teszi, hogy két érték:

- **Bejegyzés:** Ha a kérés a HTTP-bejegyzés használt
- **Első:** Ha a kérés HTTP GET használt

    Példa:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** és **d:Namespace** – a csomópont a névtér az XML-fájl importálása (URI végpont) függvény által visszaküldött definiált mutatja be. Az XML-fájl a kódmentes szolgáltatás által visszaküldött a tartalom, a függvény által visszaadott megkülönböztetéséhez névtér tetszőleges számú tartalmazhat. **Az összes alábbi névtér, ha d:Map vagy d:Match XPath-lekérdezésekben használható kell venni.** A d:Namespaces csomópont d:Namespace csomópontok beállítása/listáját tartalmazza. Mindegyiket megjeleníti a kódmentes szolgáltatás válasz használt egy névtér. Az attribútum d:Namespace csomópont a következők:

-   **d:Prefix:** Az előtag a névtér az XML eredménye a szolgáltatást, például f:FirstName, f:LastName, ahol f az előtag a látható módon.
- **d:Uri:** Az eredmény dokumentumban használt névtér teljes URI. Az érték, amely az előtag megoldódott-e a futásidőben képviseli.

**d:ErrorHandling** *(Nem kötelező)* - csomópont hibakezelés vonatkozó feltételeket tartalmaz. A feltételek mindegyike érvényesít a rendszer a tartalomszolgáltató szolgáltatás által visszaadott eredménye. Ha egy feltétel megegyezik a javasolt HTTP hiba jelenik meg hibaüzenet a végfelhasználói adja vissza.

**d:ErrorHandling** *(Nem kötelező)* , és **d:Condition** *(nem kötelező)* - egy feltétel csomópontot a egy feltétel, hogy be van jelölve a tartalomszolgáltató szolgáltatás által visszaadott eredménye a tárolja. A **szükséges** attribútumokat a következők:

- **d:Match:** XPath-kifejezés, amely ellenőrzi, hogy egy adott csomópont érték szerepel a tartalomszolgáltató kimeneti XML-e. Az XPath szemben a kimenet fut, és térjen vissza IGAZ, ha a feltétel hiányos a hol.van vagy hamis.
- **d:HttpStatusCode:** A HTTP-állapotkód, amely a visszaadandó által piactér abban az esetben a feltételnek megfelelő. Piactér signalizes hibák HTTP állapot kódok – a felhasználó számára. HTTP állapot kódok listáját érhetők el a http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** A hibaüzenet, a függvény által visszaadott –, a HTTP állapotkód – szeretné a végfelhasználói. Rövid hibaüzenetet, amely nem tartalmaz olyan titkos kulcsok kell.

**d:Title** *(Nem kötelező)* - lehetővé teszi, hogy azt mutatja be, a cím, a függvény. A cím érték érkezik

- A térkép nem kötelező attribútum (xpath) amely meghatározza, hogy hol találhatók a címet a válasz, a szolgáltatási kérelem – által eredményül adott.
- – Vagy – a megadott érték a csomópont címére.

**d:Rights** *(Nem kötelező)* - (például copyright) tartalomvédelmi társított a függvény. Az érték a tartalomvédelmi származik:

- A térkép nem kötelező attribútum (xpath) amely meghatározza, hogy hol találhatók a a tartalomvédelmi a válasz, a szolgáltatási kérelem – által eredményül adott.
-   – Vagy – a megadott érték a csomópont jogok.

**d:Description** *(Nem kötelező)* - egy rövid leírást a függvény. A leírás érték érkezik

- A térkép nem kötelező attribútum (xpath) amely meghatározza, hogy hol találhatók a leírás a válasz, a szolgáltatási kérelem – által eredményül adott.
- – Vagy – a megadott érték a csomópont leírását.

**d:EmitSelfLink** - *Lásd a fenti példa "FunctionImport"Lapozás"keresztül visszaadott adatok"*

**d:EncodeParameterValue** - OData választható bővítmény

**d:QueryResourceCost** - OData választható bővítmény

**d:Map** - OData választható bővítmény

**d:Headers** - OData választható bővítmény

**d:Headers** - OData választható bővítmény

**d:Value** - OData választható bővítmény

**d:HttpStatusCode** - OData választható bővítmény

**d:ErrorMessage** - OData választható bővítmény

## <a name="parameter-node"></a>Paraméter csomópontot.

A csomópont jelöl, amely a URI-sablon részeként van téve egy paraméter kérése a szervezet a FunctionImport csomópontot a megadott /.

Egy nagyon hasznos dokumentum adatlapján a "A paraméter eleme" csomópont az megtalálható [Itt](http://msdn.microsoft.com/library/ee473431.aspx) (használata az **Egyéb verzió** legördülő jelölje ki egy másik verzióját, ha szükséges, a dokumentáció megtekintése). *Példa:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Paraméter attribútum | Szükség | Érték |
|----|----|----|
| név | igen | A paraméter neve. Kis-és nagybetűk!  És nagybetű különbözik a BaseUri elemet meg. **Példa:**`<Property Name="IsDormant" Type="Byte" />` |
| Típus | igen | A paraméter típusa. Az érték egy **EDMSimpleType** vagy összetett típusú, amely a modell hatálya alá kell lennie. További tudnivalókért lásd: a "6 paraméter/tulajdonság támogatott típusú".  (Kis-és nagybetűk! Első karaktere nagybetűsre, többi kisbetű.)  Lásd még:, [elvi modell típusok (CSDL)] [MSDNParameterLink]. **Példa:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Mód | nem | **A**, ki vagy be/ki attól függően, hogy a paraméter egy beviteli, kimeneti vagy bemeneti és kimeneti paraméter. (Csak az "IN" érhető el a Microsoft Azure piactéren.) **Példa:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | nem | A maximális hossza paraméter engedélyezett. **Példa:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Pontosság | nem | A paraméter pontosság. **Példa:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Méretarány | nem | A paraméter beosztásának. **Példa:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Az alábbiakban a CSDL specifikációja felvette, amelynek tulajdonságait:

| Paraméter attribútum | Leírás |
|----|----|
| **d:Regex** *(Nem kötelező)* | A bemeneti érték a paraméter érvényesítéséhez használt regex nyilatkozat. Az érték nem fogadja el, ha a bemeneti érték nem egyezik meg a kimutatás. Ez lehetővé teszi, hogy meg kell adnia is lehetséges értékek, például ^ [0 – 9-es] +? $ a Tevékenységfrissítések engedélyezése csak a számokat. **Példa:** "< paraméternév ="név"mód ="A"típus ="Karakterlánc"d: Nullable ="false értéket"d:Regex =" ^ [a-hamza-Z] * $"d:Description ="A, amely nem tartalmaz szóközöket vagy nem alfa nem angol nyelvű karakterek neve"d:SampleValues ="György|János|Balázs|Zsolt "/ >" |
| **d:enum** *(Nem kötelező)* | A függőleges vonás elválasztott értéklista érvényes a paraméter. Milyen típusú értékeket kell megfelelnek a megadott a paramétert. Példa: "angol|metrikus|nyers`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< paraméternév = "Időtartam" típus = "Karakterlánc" mód = "A" Nullable = "igaz" d:Enum = "1 év|5years|10years "/ >" |
| **d: Nullable** *(Nem kötelező)* | Lehetővé teszi, hogy meghatározása, hogy paraméter lehet null. Az alapértelmezett érték: igaz. Paraméterek, az elérési út a URI sablonban részeként elérhető azonban nem lehet üres. Ha az attribútum értéke hamis paraméterértékek – a felhasználó által érvényét veszti. **Példa:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Nem kötelező)* | Egy minta érték egy megjegyzés az ügyfélnek felhasználói felület megjelenítéséhez.  Több értéket felvétele a függőleges vonás tagolt listáját, azaz használatával lehetséges "egy|b|c` **Example:** `< paraméternév = "BikeOwner" típus = "Karakterlánc" mód = "A" d:SampleValues = "György|János|Balázs|Zsolt "/ >" |

## <a name="entitytype-node"></a>EntityType csomópontot.

A csomópont jelöli a piactérről a végfelhasználó visszaküldött típusok közül. A hozzárendelés a eredményéből a végfelhasználói kerülnek vissza értékeket a tartalomszolgáltató szolgáltatás által visszaküldött is tartalmaz.

A csomópont részleteket vannak megtalálható [Itt](http://msdn.microsoft.com/library/bb399206.aspx) (használata az **Egyéb verzió** legördülő lista kijelölése egy másik verzióját, ha szükséges, a dokumentáció megtekintése.)

| Attribútumnév | Szükség | Érték |
|----|----|----|
| név | igen | A szervezet-típus nevére. **Példa:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | nem | Másik személy típusú, amely meghatározza a személy típusú az Alap típusú neve. **Példa:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Az alábbiakban a CSDL specifikációja felvette, amelynek tulajdonságait:

**d:Map** – szemben a szolgáltatás kimenet végrehajtása az XPath-kifejezés. Azon a feltételezésen itt, hogy a szolgáltatás eredménye tartalmaz egy sor olyan elemeket, amelyek ismételje meg, mint például egy ATOM hírcsatorna where meg ismétlődő bejegyzés csomópontok van. Több rekordot mindegyike csomópontok ismétlődő tartalmaz. Az XPath majd mutasson az adott ismétlődő csomópontjának a tartalomszolgáltató szolgáltatás eredményt az adott bejegyzéshez értékkel rendelkező van megadva. Példa a kimeneti a szolgáltatásból:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

XPath-kifejezés volna /foo kell/sáv, mivel az egyes a sáv csomópontot a kimeneti ismétlődő csomópont és a végfelhasználói visszakerül a tényleges elemeket tartalmaz.

**Kulcs** – Ez az attribútum piactér figyelmen kívül hagyja. TÖBBI alapú webszolgáltatásokhoz, általában nem jelenítik meg az elsődleges kulcs.


## <a name="property-node"></a>Csomópont tulajdonság

A csomópont a rekord egy tulajdonságot tartalmazza.

A csomópont részleteket talál a [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (használata az **Egyéb verzió** legördülő lista kijelölése egy másik verzióját, ha szükséges, a dokumentáció megtekintése.) *Példa:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| Attribútumnév | Szükséges | Érték |
|----|----|----|
| név | igen | A tulajdonság neve. |
| Típus | igen | A tulajdonság értékét a típusát. A tulajdonság értékét típusa egy **EDMSimpleType** vagy összetett típusú (egy teljesen minősített neve jelölt), amely a modell hatálya alá kell lennie. További tudnivalókért lásd: elvi modell típusok (CSDL). |
| Nullable | nem | **Igaz** (az alapértelmezett érték) vagy **hamis** attól függően, hogy a tulajdonságot beállíthatja, hogy a null érték. Megjegyzés: A [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) névtér jelölt CSDL verziójában egy összetett típusú tulajdonság kell rendelkeznie Nullable = "hamis értékre". |
| DefaultValue | nem | A tulajdonság az alapértelmezett értéket. |
|MaxLength | nem | A maximális hossza a tulajdonság értékét. |
| FixedLength | nem | **Igaz** vagy **hamis** attól függően, hogy a tulajdonság értékét fiexed hosszúságú karakterlánc szeretne tárolni. |
| Pontosság | nem | A maximális számú számjegyre, amely megőrzi a numerikus értéket hivatkozik. |
| Méretarány | nem | Maximális számú tizedesjegyre őrizzen meg a program a numerikus értéket. |
| Unicode | nem | **Igaz** vagy **hamis** attól függően, hogy kell-e a tulajdonság értékét Unicode karakterláncként tárolja. |
| Rendezési sorrend | nem | Egy karakterlánc, amely meghatározza a rendezési sorrend az adatforrás használható. |
| ConcurrencyMode | nem | **Nincs lehetőség** (az alapértelmezett érték) vagy a **rögzített**. Ha az érték a **rögzített**van beállítva, a tulajdonság értékét az optimista feldolgozási ellenőrzések használható. |

A további attribútumok a CSDL specifikációja felvett a következők:

**d:Map** – szemben a szolgáltatás végrehajtott XPath-kifejezés kimeneti, és az első karaktertől a kimenet egy tulajdonságot. A megadott XPath-kifejezés az ismétlődő csomópontot, a EntityType csomópont XPath jelölt viszonyított van. Érdemes emellett is megadhat egy abszolút XPath kimeneti statikus erőforrás beleértve minden kimeneti csomópontok, mint például egy szerzői jogi nyilatkozat, amely csak egyszer található az eredeti szolgáltatásban, de kell lennie az OData-eredményt ad a sorok minden engedélyezni. Példa a szolgáltatásból:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

XPath-kifejezés itt úgy juthat az baz0 csomópontot a tartalomszolgáltató szolgáltatás ./bar/baz0 lenne.

**d:CharMaxLength** - karakterlánc típusú, adja meg a maximális hossza. Lásd: DataService CSDL példa

**d:IsPrimaryKey** - azt jelzi, ha az oszlop az elsődleges kulcs a tábla nézetben. Lásd DataService CSDL példát.

**d:isExposed** – határozza meg, ha a táblázat séma van téve (általában igaz). Lásd: DataService CSDL példa

**d:IsView** *(Nem kötelező)* - igaz, ha ez alapján egy táblázat, hanem egy nézetet.  Lásd: DataService CSDL példa

**d:Tableschema** – lásd: DataService CSDL példa

**d:ColumnName** - a táblázat nézetben oszlopának a nevét.  Lásd: DataService CSDL példa

**d:IsReturned** - a logikai érték, amely azt határozza meg, ha a szolgáltatás közzététele ezt az értéket, az ügyfél számára.  Lásd: DataService CSDL példa

**d:IsQueryable** - a logikai érték, amely azt határozza meg, ha az oszlop egy adatbázis-lekérdezés használható.   Lásd: DataService CSDL példa

**d:OrdinalPosition** – az a oszlop numerikus pozícióját megjelenését, x, az a táblázat vagy a nézetet, ahol x az 1-től a táblázatban lévő oszlopok számával.  Lásd: DataService CSDL példa

**d:DatabaseDataType** - az adatbázisban, az oszlop adattípusát, tehát írja be az SQL-adatokat. Lásd: DataService CSDL példa

## <a name="supported-parametersproperty-types"></a>Támogatott paraméterek/tulajdonság típusai
Az alábbiakban a támogatott típusú paramétereket és tulajdonságait. (Kis-és nagybetűk)

| Egyszerű típusokhoz | Leírás |
|----|----|
| NULL | Érték hiánya jelöli. |
| Logikai érték | Bináris értékű logika matematikai fogalmának jelöli.|
| Bájt | Alá nem írt 8 bites egész|
|Dátum és idő| Dátum és idő jelöli kezdve a 12:00:00 éjfél, január 1, 1753 I.E. értékekkel 11:59:59 óráig, December 9999 I.E. keresztül|
|Decimális | A rögzített pontosságú és skála numerikus értékek jelöli. Ez a típus leírhatja numerikus érték a negatív 10 ^ 255 + 1 billentyűkombinációt, ha pozitív 10 ^ 255 -1|
| Dupla | Lebegőpontos szám ± 2.23e-308 keresztül + 1, 79E közelítő cellatartomány értékek megjelenítésére alkalmas 15 számjegy pontosságú jelöli +308. **Decimális használata Exel exportálás probléma miatt**|
| Egyetlen | Lebegőpontos szám ± 1.18e-38 keresztül + 3.40e közelítő cellatartomány értékek megjelenítésére alkalmas 7 jegy pontossággal jelöli + 38|
|Globálisan egyedi azonosítója |A 16 bájtos az egyedi azonosító (128 bites) érték |
|Int16|Aláírt 16 bites egész jelöli. |
|Int32|Aláírt 32 bites egész jelöli. |
|Int64|Aláírt 64 bites egész jelöli. |
|Karakterlánc | Rögzített vagy változó-hosszúságú karakter adat |


## <a name="see-also"></a>Lásd még:
- Ha érdeklik a teljes OData-megfeleltetés folyamat és célját ismertetése, olvassa el az Ez a cikk [Adatok szolgáltatás OData-hozzárendelés](marketplace-publishing-data-service-creation-odata-mapping.md) , tekintse át a definíciókat, szerkezetek és az utasításokat.
- Ha érdeklik a példák megtekintésével, olvassa el a jelen cikk lásd: minta kód és értelmezhető a mezőkódok szintaxisa és a helyi [Adatok szolgáltatás OData-leképezés példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) .
- Szeretne visszatérni az Azure piactéren elérhető adatok szolgáltatás közzététele előírt elérési útját, olvassa el az [Adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md)Ez a cikk.
