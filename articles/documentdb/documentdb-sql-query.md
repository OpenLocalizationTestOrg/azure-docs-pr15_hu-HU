<properties 
    pageTitle="SQL-szintaxisa és az SQL-lekérdezés DocumentDB az |} Microsoft Azure" 
    description="SQL-szintaxisa adatbázis fogalmak és SQL-lekérdezések találhat DocumentDB, NoSQL adatbázis. A JSON-lekérdezési nyelv DocumentDB az SQL is használható." 
    keywords="SQL-szintaxisa, sql-lekérdezés, sql-lekérdezések, json-lekérdezési nyelv, adatbázis fogalmak és sql-lekérdezések összesítő függvények"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-lekérdezés és a DocumentDB SQL-szintaxisa
Microsoft Azure DocumentDB lekérdezésekor dokumentumok SQL (Structured Query Language) használata a JSON-lekérdezési nyelvet támogat. DocumentDB, hogy valóban séma nélkül. Alapján a lekötési JSON az adatmodellhez közvetlenül a adatbázismotor belül itt Automatikus indexelés JSON dokumentumok explicit séma vagy a másodlagos indexek létrehozása nélkül. 

A lekérdezési nyelv tervezésekor az DocumentDB volt két célok szem előtt:

-   Egy új JSON-lekérdezési nyelv inventing, hanem azt szeretett volna SQL támogatja. SQL egyike, a már jól ismert és a népszerű lekérdezés nyelvek. DocumentDB SQL hivatalos programozási modellt gazdag lekérdezések JSON dokumentumok keresztül biztosít.
-   Adatbázisként JSON dokumentum végre tud hajtani a JavaScript közvetlenül az adatbázis-vezérlő hogy szeretett volna alapját JavaScript programozási modell használata a lekérdezési nyelv. A DocumentDB SQL gyökerezik JavaScript-típus rendszer, a kifejezés értékelése és a hívás függvény. Ez az-bekapcsolása nyújt a természetes programozási modell relációs előrejelzések, hierarchikus navigációs JSON dokumentumokat, saját illesztés, térbeli lekérdezések és a felhasználó által definiált függvények (UDF) írt teljes egészében JavaScript egyéb szolgáltatások között a hívás keresztül. 

Úgy véli, hogy ezek a funkciók billentyű lenyomásával csökkentése a súrlódás az alkalmazás és az adatbázis között, roppant Fejlesztőeszközök is elérhető.

Javasoljuk, hogy az első lépések a következő videót, ahol Aravind Ramachandran DocumentDB's lekérdezésekor funkciók jeleníti meg, és a [Lekérdezés játszótéri](http://www.documentdb.com/sql/demo), ahol próbálja ki az DocumentDB és SQL-lekérdezések futtatása szemben az adatkészlet megtalálhatók.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Ezt követően lépjen vissza Ez a cikk Ha lássuk először egy SQL-lekérdezés oktatóprogram, amelyek végigvezetik Önt néhány egyszerű JSON-dokumentumok és az SQL-parancsok az.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>SQL-parancsok DocumentDB az első lépések
DocumentDB SQL a munkahelyén, nézzük néhány egyszerű JSON dokumentumok kezdődő és című végigvezetik néhány egyszerű lekérdezéseket. Fontolja meg a következő két JSON dokumentumok két család kapcsolatos. Fontos tudni, hogy a DocumentDB, azt nem hozhat létre bármely sémák vagy a másodlagos tárgymutatók használata explicit módon. Egyszerűen szükség beszúrása DocumentDB webhelycsoportra JSON dokumentumokat, és ezt követően a lekérdezés. Itt van még egy egyszerű JSON dokumentumadatok a Andersen család, a szülők, gyermekek (és azok Hobbiállatok), a cím és a regisztrációs. A dokumentum karakterláncok, számokat, logikai értékek, tömbök és beágyazott tulajdonságok rendelkezik. 

**Dokumentum**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Az alábbiakban a második dokumentumokat egyetlen különbség – `givenName` és `familyName` helyett használt `firstName` és `lastName`.

**Dokumentum**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Most már kipróbálása lehet áttekinteni néhány DocumentDB SQL kulcs tulajdonságát adat néhány lekérdezéseket. Például az alábbi lekérdezés ad eredményül a dokumentumok hol az azonosító mezőt megfelelő `AndersenFamily`. Mivel ez éppen a `SELECT *`, a lekérdezés eredményét a teljes dokumentum JSON:

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Tegyük fel, az esetet, ahol kell formáznia a JSON eredménye egy másik alakzat. Ezt a lekérdezést egy új JSON-objektum neve és a város, két kijelölt mezővel projektek, amikor a város ' nevével megegyező nevű, mint az állapot. Ebben az esetben a "NY, NY" megegyezik.

**Lekérdezés**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Eredmény**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


A következő lekérdezés eredménye gyermekek minden megadott név a család azonosítójú megfelelő `WakefieldFamily` tartózkodási a listán szereplő város szerint rendezett.

**Lekérdezés**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Eredmény**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Azt szeretné felhívhatja a figyelmet a DocumentDB lekérdezésnyelv keresztül a példák azt láthatta, az eddigi néhány méltó szempontokat:  
 
-   Mivel DocumentDB SQL működése JSON értékek, sorok és oszlopok helyett szervezetek alakú fa foglalkozik. Emiatt a nyelv lehetővé teszi például olvassa el a fa bármilyen tetszőleges mélységű csomópontjainak `Node1.Node2.Node3…..Nodem`, a két kijelző hivatkozását hivatkozó relációs SQL hasonló `<table>.<column>`.   
-   A structured query language a séma nélküli adatokkal dolgozik. Emiatt a típus rendszer kell kötni dinamikusan. Ugyanabban a kifejezésben sikerült hozam különféle különböző dokumentumokon. A lekérdezés eredményét JSON érvényes érték, de előfordulhat, hogy egy rögzített séma kell.  
-   DocumentDB csak szigorú JSON dokumentumok használatát támogatja. Ez azt jelenti, hogy a rendszer típusa és kifejezések csak JSON típusok foglalkozik. A [JSON specifikációja](http://www.json.org/) további részletekért olvassa el.  
-   Egy DocumentDB gyűjteménye a JSON dokumentumok séma ingyenes tároló. Az adatok szervezetek belül, és a webhelycsoport dokumentumai között a kapcsolatokat implicit módon rögzítésének biztonsági és nem elsődleges kulcs és idegen kulcs kapcsolatokat. Ez a értékű a jelen cikkben ismertetett belüli-dokumentum illesztések alapján mutató fontos eleme.

## <a name="documentdb-indexing"></a>Az indexelés DocumentDB

Azt a DocumentDB SQL-szintaxisa foglalkozni, mielőtt a DocumentDB az indexelési tervezés felfedezése érdemes. 

Az adatbázis tárgymutatók célja lekérdezések a különböző űrlapok és alakzatokat (például a Processzor- és bemeneti és kimeneti) minimális erőforrás-felhasználás szolgáló során jó átvitel és alacsony késés kezeléséről. Gyakran az adatbázis lekérdezése a megfelelő index választása és elő kell készítenie sok kísérletezés. Ezt a megközelítést séma nélküli adatbázisok, ahol szigorú séma nem felel meg az adatokat, és gyorsan követi bonyolulttá jelent. 

Emiatt az indexelő alrendszer DocumentDB tervezésekor be van állítva a következő célok:

-   Dokumentumok tárgymutató nélkül séma: az indexelő alrendszer megkövetelése séma információkat, vagy nem minden séma feltételezéseket a dokumentumok ellenőrizze. 

-   A hatékony, gazdag hierarchikus és relációs lekérdezések támogatja: az index támogatja a DocumentDB lekérdezésnyelv hatékony, hierarchikus és relációs előrejelzések-támogatással.

-   Írások tartós mennyiségig in face of egységes lekérdezések támogatása: A magas írási átviteli munkaterhelésekből egységes lekérdezések, az index frissítése fokozatosan, hatékonyan és online írások tartós mennyiségig szemben. Az egységes index frissítése elengedhetetlen a lekérdezéseket, amelyben a felhasználó beállítva a szolgáltatás konzisztencia szintre szolgálnak.

-   Több elem bérleti támogatása: adva a alapú foglalás modell erőforrás irányítási bérlők között, index a frissítések megtörténik a rendszer-erőforrásokat (a Processzor, a memória és a bemeneti és kimeneti műveletek / szekundum) replika projektenként a költségvetés keretei között. 

-   Tárterület hatékonyság: költség hatékonyság, az index a lemezen tárolási terhelést korlátos és kiszámítható. Ennek oka kulcsfontosságú DocumentDB lehetővé teszi, hogy a fejlesztői, hogy alapú költség kompromisszumok beleszámítva a lekérdezési teljesítmény viszonyított index között.  

Olvassa el a [DocumentDB minták](https://github.com/Azure/azure-documentdb-net) MSDN konfigurálja az indexelési házirendet gyűjtemény ábrázoló minták. Vegyük most foglalkozni az DocumentDB SQL-szintaxisa részleteit.


## <a name="basics-of-a-documentdb-sql-query"></a>Egy DocumentDB SQL-lekérdezés alapjai
Minden lekérdezés áll a SELECT záradékban és a választható KEZDŐ és a WHERE záradék per ANSI SQL szabványoknak. A szokásos mindegyik lekérdezéshez, a FROM záradékban a forrás megjelenik a listán. Kattintson a szűrő a WHERE záradékban érvényesül JSON dokumentumokat csak egy részhalmazát beolvasni a forrás. Végül a SELECT záradék a kért JSON értékeket a listában válassza a Project használják.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM záradék.
A `FROM <from_specification>` záradék a lépés nem kötelező, kivéve, ha a forrás szűrve vagy tervezett később a lekérdezésben. Ez a záradék célja, adja meg az adatforrás, amelyen a lekérdezés csak akkor működnek. A teljes webhelycsoport gyakran a forrás, de inkább egy adhat meg a webhelycsoport egy részét. 

A lekérdezés például `SELECT * FROM Families` azt jelzi, hogy a teljes családok gyűjtemény a forrás számba veheti a webhely keresztül. Egy speciális azonosítóval legfelső szintű használható jelenítik meg a webhelycsoport helyett a webhelycsoport nevét használja. Az alábbi lista tartalmazza a kényszerített lekérdezése szabályok:

- A webhelycsoport lehet aliassal, mint például `SELECT f.id FROM Families AS f` vagy egyszerűen csak `SELECT f.id FROM Families f`. Itt `f` az egyenértékű `Families`. `AS`az alias választható kulcsszót a azonosítója lesz.

-   Megjegyzés: Ez azt egyszer aliassal, az eredeti forrás nem kötelező. Ha például `SELECT Families.id FROM Families f` érvénytelen szintaktikai szempontból óta az azonosító "Családok" a továbbiakban nem oldható.

-   Az összes tulajdonság, amelyre hivatkozni kíván kell teljesen minősítettnek kell lennie. Szigorú séma betartása hiányában a lép érvénybe bármely kétértelmű kötések elkerülése érdekében. Ezért `SELECT id FROM Families f` érvénytelen szintaktikai szempontból óta a tulajdonság `id` számára nem kötelező.
    
### <a name="sub-documents"></a>Alszint dokumentumok
A forrásból is csökkenthető egy kisebb részét. Például csak a minden szövegben alszint fa számbavétele, hogy a alszint legfelső szintű majd válhat forrás, az alábbi példában látható módon.

**Lekérdezés**

    SELECT * 
    FROM Families.children

**Eredmény**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

A fenti példa tömböt forrásaként használt, miközben egy objektumot is is használható a forrás, amely a következő példában látható. Bármely érvényes JSON érték (nem meghatározatlan), amely megtalálható a forrás veszi figyelembe a közzététel a lekérdezés eredményét. Ha nincs telepítve az egyes családok egy `address.state` érték, a lekérdezés eredményében kimarad.

**Lekérdezés**

    SELECT * 
    FROM Families.address.state

**Eredmény**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>A WHERE záradék
A WHERE záradék (**`WHERE <filter_condition>`**) nem kötelező. Azt adja meg, amelyeknek a mezőértékei, a JSON-dokumentumokat a forrás megadott meg kell felelniük annak érdekében, hogy az eredmény része lesz. Bármely JSON dokumentum kiértékelésének eredménye "igaz" figyelembe kell venni az eredmény a megadott feltételeknek. A WHERE záradék az index réteg által használt forrás dokumentumokat, amelyek lehetnek az eredményben abszolút legkisebb részhalmazát meghatározásához. 

Az alábbi lekérdezés kéri egy név tulajdonság, amelynek az értéke tartalmazó dokumentumok `AndersenFamily`. A name tulajdonság nincs telepítve a bármely más dokumentumot, vagy ha az érték nem felel meg `AndersenFamily` nem szerepel. 

**Lekérdezés**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Az előző példában egy egyszerű egyenlő lekérdezés mutatott. DocumentDB SQL számos skaláris kifejezés. A leggyakrabban használt bináris és a Unáris. A JSON Forrásobjektum tulajdonság hivatkozásait is érvényes kifejezés. 

Az alábbi bináris operátorok jelenleg támogatott és lekérdezésekben is használható, ahogy az alábbi példák:  
<table>
<tr>
<td>Számtani</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitenkénti</td>    
<td>|}, &, ^, <<, >>, >>> (nulla kitöltés jobb shift) </td>
</tr>
<tr>
<td>Logikai</td>
<td>ÉS, VAGY NEM</td>
</tr>
<tr>
<td>Összehasonlítás</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Karakterlánc</td> 
<td>|| (ÖSSZEFŰZ)</td>
</tr>
</table>  

Most tekintsünk át egy bizonyos bináris operátorokkal lekérdezéseket.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Az unáris operátor +,-, ~, és nem is támogatottak, és lekérdezések belül is használható, az alábbi példában látható módon:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Nemcsak a bináris és unáris operátor tulajdonság hivatkozások is használhatók. Ha például `SELECT * FROM Families f WHERE f.isRegistered` a JSON-dokumentumot tartalmazó a tulajdonság adja eredményül `isRegistered` hol a tulajdonság értéke megegyezik a JSON `true` értéket. Minden más érték (hamis, NULL értékű, nem meghatározott, `<number>`, `<string>`, `<object>`, `<array>`stb) az eredményből kizárásával forrásdokumentumán vezet. 

### <a name="equality-and-comparison-operators"></a>Egyenlő és összehasonlító operátorok
A következő táblázat mutatja az eredményt egyenlő-összehasonlító táblázat DocumentDB SQL bármely JSON két típusa között.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>M</strong>
         </td>
         <td valign="top">
            <strong>Nem meghatározott</strong>
         </td>
         <td valign="top">
            <strong>NULL</strong>
         </td>
         <td valign="top">
            <strong>Logikai érték</strong>
         </td>
         <td valign="top">
            <strong>Szám</strong>
         </td>
         <td valign="top">
            <strong>Karakterlánc</strong>
         </td>
         <td valign="top">
            <strong>Objektum</strong>
         </td>
         <td valign="top">
            <strong>Tömb</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nem meghatározott<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>NULL<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
            <strong>oké</strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Logikai érték<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
            <strong>oké</strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Szám<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
            <strong>oké</strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Karakterlánc<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
            <strong>oké</strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objektum<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
            <strong>oké</strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Tömb<strong>
         </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
Nem meghatározott </td>
         <td valign="top">
            <strong>oké</strong>
         </td>
      </tr>
   </tbody>
</table>

Az összehasonlítás más operátorokat, mint >, > =,! =, < és < =, az alábbi szabályok vonatkoznak:   

-   Különböző típusú összehasonlító meghatározatlan eredményez.
-   Két objektumok vagy két összehasonlítása tömbök nem meghatározott eredményez.   

Ha a szűrés skaláris kifejezés eredménye meghatározatlan, a megfelelő dokumentum nem lesz része az eredményben, mivel az nem meghatározott logikailag feleltetni nem "igaz".

### <a name="between-keyword"></a>Kulcsszó között
A BETWEEN kulcsszó az ANSI SQL például értéktartományok lekérdezéseket Express is használhatja. KÖZÖTT használható karakterláncok vagy szám alapján.

A lekérdezés például minden családi dokumentum, amelyben az első alárendelt minőségű 1-5 (mindkettő végpontokat is beleértve) között van adja eredményül. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Nem kedvelik-e az ANSI SQL is használhat a BETWEEN záradék a FROM záradékban, például a következő példában.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Így gyorsabban lekérdezés-végrehajtási lapok ne felejtse el a tartomány index típusa elleni bármely numerikus tulajdonságok/elérési út a BETWEEN záradékban szűrt használó indexelési házirend létrehozása. 

A fő DocumentDB és az ANSI SQL BETWEEN használata között különbség, hogy akkor is express vegyes típusú tulajdonságok tartomány lekérdezéseket – például lehet, hogy egy szám (5) a "kategória" egyes dokumentumok és karakterláncok mások ("grade4"). Ebben az esetben például a JavaScript, két különböző típusú eredmény "nem meghatározott" és a dokumentum összehasonlítása kimarad.

### <a name="logical-and-or-and-not-operators"></a>Logikai (és, vagy, és nem) operátorok
Logikai operátorok logikai értékek vonatkozni fognak. Az alábbi operátorok logikai igazság táblázatokban az alábbi táblázatban látható.

VAGY|Igaz|Hamis|Nem meghatározott
---|---|---|---
Igaz|Igaz|Igaz|Igaz
Hamis|Igaz|Hamis|Nem meghatározott
Nem meghatározott|Igaz|Nem meghatározott|Nem meghatározott

ÉS|Igaz|Hamis|Nem meghatározott
---|---|---|---
Igaz|Igaz|Hamis|Nem meghatározott
Hamis|Hamis|Hamis|Hamis
Nem meghatározott|Nem meghatározott|Hamis|Nem meghatározott

NEM|  |
---|---
Igaz|Hamis
Hamis|Igaz
Nem meghatározott|Nem meghatározott

### <a name="in-keyword"></a>A kulcsszó
Ellenőrizze, hogy egy megadott értéket megegyezik-e bármelyik értékre abban a listában az IN kulcsszó használható. Például ezt a lekérdezést ad vissza az összes családi dokumentumok ahol azonosítója egyik "WakefieldFamily" vagy "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Ez a példa ad vissza minden dokumentum ahol állapotát a megadott értékek közül.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternáris (?), és operátorokat összetevőjében (?)
Az Ternáris és összetevőjében operátorok feltételes kifejezéseket, hasonlóan, mint például a C# és JavaScript népszerű programnyelv létrehozásához használható. 

A Ternáris (?) operátorral nagyon hasznos lehet, új JSON tulajdonságai menet kiszámításakor. Például most is írhat a osztály szintek osztályozásához például kezdő/betétlap/speciális alább látható módon emberi olvasható űrlapba lekérdezések.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Az az operátor az alábbi lekérdezés például hívásokat is ágyazhatók egymásba.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Más lekérdezés operátorokkal, ha a feltételes kifejezésben hivatkozott tulajdonságainak hiányoznak minden dokumentumban, vagy ha éppen képest fájltípusok eltérőek, majd ezeket a dokumentumokat kimarad a lekérdezés eredményei között.

Az egyesítés (?) operátor használható hatékony keresésének (más néven egy tulajdonságot jelenlétét van megadva) a dokumentumban. Ez akkor hasznos, ha a félig strukturált lekérdezését, illetve vegyes típusú adatok. Például a lekérdezés visszaadja a "Vezetéknév" Ha jelen van, vagy a "Vezetéknév" Ha ez nem bemutató.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Hozzáférő jegyzett tulajdonság
Is elérheti a jegyzett tulajdonság operátorral tulajdonságok `[]`. Ha például `SELECT c.grade` és `SELECT c["grade"]` között nem tesz különbséget. E szintaktikai akkor hasznos, ha a szükséges szóközöket tartalmaz, speciális karaktereket, vagy megoszthatja neve megegyezik egy SQL-kulcsszó vagy foglalt szó történik tulajdonság escape.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT záradék.
A SELECT záradékban (**`SELECT <select_list>`**) kötelező, és adja meg, milyen értékeket lehet beolvasni a lekérdezésből, hasonlóan a ANSI-SQL nyelvben. Azon részhalmazát jelenti, hogy van szűrve a forrás dokumentumok fölött át az alakzatot a kivetítés szakasz, ahol a megadott JSON értékek beolvasható, és egy új JSON-objektum összeállítás, minden beviteli átadott azt az alakzatot. 

A következő példa bemutatja egy tipikus VÁLASZTÓ lekérdezést. 

**Lekérdezés**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Beágyazott tulajdonságai
A következő példában a két beágyazott tulajdonságok kivetítése azt `f.address.state` és `f.address.city`.

**Lekérdezés**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Vetítés JSON kifejezések is támogat, az alábbi példában látható módon.

**Lekérdezés**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Tekintsük át a szerepe `$1` ide. A `SELECT` záradékot kell JSON-objektum létrehozása, és nincs kulcs van megadva, mivel használjuk implicit argumentum változóinak nevében kezdődő `$1`. Például ez a lekérdezés visszaadja a két implicit argumentum változó, címkézett `$1` és `$2`.

**Lekérdezés**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Alias
Most vegyük kiterjesztése a fenti példában explicit alias az értékeket. Ez a kulcsszó alias használható. Ne feledje, hogy nem kötelező, miközben a bemutatót tart, mint a második értékig látható módon `NameInfo`. 

Abban az esetben, ha lekérdezés azonos nevű két tulajdonsága van, alias átnevezni az egyik vagy mindkét tulajdonságait, hogy azok is használatát a tervezett eredmény kell használni.

**Lekérdezés**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skaláris kifejezés
A SELECT záradék tulajdonság hivatkozás mellett is támogat skaláris kifejezés, például állandók, aritmetikai kifejezések, logikai kifejezések és stb. Íme például egy egyszerű "Helló, világ" lekérdezés.

**Lekérdezés**

    SELECT "Hello World"

**Eredmény**

    [{
      "$1": "Hello World"
    }]


Íme egy összetettebb példa, amely egy skaláris kifejezés használja.

**Lekérdezés**

    SELECT ((2 + 11 % 7)-2)/3   

**Eredmény**

    [{
      "$1": 1.33333
    }]


A következő példában a skaláris kifejezés eredménye egy logikai érték.

**Lekérdezés**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Eredmény**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Az objektum és tömb létrehozása
Egy másik kulcs DocumentDB SQL funkció tömb/objektumainak létrehozása. Az előző példában figyelje meg, hogy létrehozott új JSON-objektumot. Hasonlóképpen egy szerkezetét is tömbök az alábbi példák látható módon.

**Lekérdezés**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Eredmény**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>ÉRTÉK kulcsszó
Az **érték** kulcsszó megoldást JSON értéket adja eredményül. Például a alább látható módon a lekérdezés eredménye a skaláris `"Hello World"` helyett `{$1: "Hello World"}`.

**Lekérdezés**

    SELECT VALUE "Hello World"

**Eredmény**

    [
      "Hello World"
    ]


Az alábbi lekérdezés nélkül JSON értékét adja eredményül a `"address"` címke a találatok között.

**Lekérdezés**

    SELECT VALUE f.address
    FROM Families f 

**Eredmény**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

A következő példa kiterjeszti az ennek való visszatéréshez JSON egyszerű értékek (a JSON fa leveles szint) módját megjelenítéséhez. 

**Lekérdezés**

    SELECT VALUE f.address.state
    FROM Families f 

**Eredmény**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operátor
A speciális műveleti jel (*) a dokumentum Project támogatott-van. Ha használja, az egyetlen tervezett mező kell lennie. Bár hasonló lekérdezés `SELECT * FROM Families f` érvényes, `SELECT VALUE * FROM Families f ` és `SELECT *, f.id FROM Families f ` nem érvényesek.

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmény**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>FELSŐ operátor
A lekérdezés értékek számának korlátozása a felső kulcsszó használható. FELSŐ együtt a ORDER BY záradék használata esetén az eredményhalmaz korlátozódik, az első N számú rendezett értéket; Ellenkező esetben az eredmény eredmények első N számú nem meghatározott sorrendben. A legjobb módszer a SELECT utasításban mindig használata ORDER BY záradék a TOP záradék. Ez a csak úgy lehet előre jelezheti a legfelső sorok érinti. 


**Lekérdezés**

    SELECT TOP 1 * 
    FROM Families f 

**Eredmény**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

FELSŐ is használható, egy konstans érték (fent látható), vagy a paraméteres lekérdezésekkel változó érték. További információra kíváncsi tanulmányozza az alábbi paraméteres lekérdezések.

## <a name="order-by-clause"></a>ORDER BY záradék
Például az ANSI-SQL nyelvben is választható Order By záradék lekérdezése közben. A záradék a sorrendben, amelyben eredmények lesz visszakeresve megadása nem kötelező növekvő vagy CSÖKKENŐ argumentumot tartalmazhat. Részletesebb megközelítésű Order By olvassa el az [DocumentDB sorrend szerint forgatókönyv](documentdb-orderby.md).

Íme például egy lekérdezést, amely beolvassa családok rezidens a város neve sorrendben.

**Lekérdezés**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Eredmény**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

És az alábbi egy lekérdezést, amely beolvassa a létrehozás dátuma, mint a alapidőpont jelző szám tárolt sorrendben családok időt, azaz január 1, 1970 a óta eltelt idő másodperc.

**Lekérdezés**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Eredmény**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Speciális adatbázis fogalmak és SQL-lekérdezések
### <a name="iteration"></a>Ismétlés
Egy új szerkezet keresztül az **IN** kulcsszó DocumentDB SQL-támogatást nyújt fölé JSON tömbök léptetés hozzá lett adva. A feladó forrás nyújt támogatást közelítését. Kezdjük a következő példa:

**Lekérdezés**

    SELECT * 
    FROM Families.children

**Eredmény**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Most Vegyük szemügyre közelítését végrehajtja a gyűjteményben gyermekek fölé egy másik lekérdezés. Megjegyzés: a különbség a kimeneti tömb. Ez a példa kettéválaszthatja-e `children` és az eredmények összeolvasztja egy egyetlen tömbképlettel be.  

**Lekérdezés**

    SELECT * 
    FROM c IN Families.children

**Eredmény**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

A további használható szeretné szűrni a tömb minden egyes belépéskor, az alábbi példában látható módon.

**Lekérdezés**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Eredmény**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Illesztés
Egy relációs adatbázisban szükséges a táblák közötti Bekapcsolódás nagyon fontos. A logikai corollary, hogy a sémák normalizált tervezéséhez. Ezzel szemben DocumentDB séma ingyenes dokumentumok nem normalizált adatok típusát foglalkozik. Ez a logikai megfelelője a "saját illesztés".

A nyelv támogató szintaxisa < from_source1 > Csatlakozás < from_source2 > Csatlakozás... Csatlakozás < from_sourceN >. Általános Ez a lásson, **N**– kockára ( **N** -értékeket tartalmazó rekord). Minden egyes sor bármelyik eleme készített az összes webhelycsoport alias léptetés fölé a megfelelő készletek értékeket tartalmaz. Más szóval ez az a halmaz vesz részt az illesztés, a teljes vektoriális szorzat.

Az alábbi példák bemutatják, hogyan működik a az ILLESZTÉS záradék. A következő példában az eredmény üres óta a forrásból származó minden dokumentum vektoriális szorzat és üres ürül.

**Lekérdezés**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Eredmény**  

    [{
    }]


A következő példában az illesztés a dokumentum legfelső szintű között van, és a `children` alszint legfelső szintű. Egy vektoriális szorzat között két JSON-objektumot. Arra, hogy gyermekekkel egy tömb nem szerepel hatékony az ILLESZTÉS óta azt, amely a gyermekek tömb egyetlen legfelső szintű foglalkoznak. Így az eredmény tartalmaz csak két eredmény, mivel a tömb a dokumentumot a vektoriális szorzat együttesen pontosan csak egy dokumentumot.

**Lekérdezés**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Eredmény**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


A következő példa bemutatja a több hagyományos illesztés:

**Lekérdezés**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Eredmény**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



A legfontosabb dolog figyelje meg, hogy a `from_source` záradék egy léptető **csatlakozni** . Igen az áramlás ebben az esetben a következőképpen történik:  

-   Bontsa ki az egyes a tömb gyermekelem **c** .
-   Az egyes gyermekelem **c** lett összeolvasztott az első lépésben az **f** -dokumentum legfelső szintű egy vektoriális szorzat vonatkoznak.
-   Végül a project a legfelső objektum **f** name tulajdonság önmagában. 

Az első dokumentum (`AndersenFamily`) csak egy gyermekelem tartalmaz, ezért a az eredményhalmaz csak egyetlen objektummá ehhez a dokumentumhoz tartozó tartalmaz. A második dokumentumban (`WakefieldFamily`) két gyermekek tartalmazza. Igen a vektoriális szorzat minden gyermek egyik az egyes ehhez a dokumentumhoz tartozó alárendelt két objektumok egyúttal így egy külön objektum hoz létre. Figyelje meg, hogy mindkét ezeket a dokumentumokat a legfelső szintű mezőket lesz azonos, ahogy az egy vektoriális szorzat várható.

Az ILLESZTÉS valós használhatóságát űrlap kockára származik, egyéb esetben projekt nehezen alakzatba határokon-termék. Ezenkívül, akkor látni fog az alábbi példában, akkor sikerült szűrheti az adatokat, ha egy sor bármelyik eleme, hogy lehetővé teszi, hogy a felhasználó választotta a kockára teljes teljesül feltétel a.

**Lekérdezés**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Eredmény**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Ebben a példában a természetes kiterjesztése az előző példában, és dupla illesztés végrehajtása. A vektoriális szorzat, megtekintheti az alábbi álösszeköttetésen kódként.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`egy gyermek, aki rendelkezik egy pet foglalja magában: Igen, az a vektoriális szorzat együttesen egy sor (1:*1*1) a család. WakefieldFamily azonban gyermekelemek két, de csak egy Gyermekszint "Jesse" nincs Hobbiállatok. Jesse 2 Hobbiállatok bár tartalmaz. Ezért a vektoriális szorzat a család együttesen 1*1*2 = 2 sorokat.

A következő példában, nincs további szűrőjének `pet`. Ez nem tartalmazza az összes kockára, ahol a pet nevem nem "Árnyékolására". Figyelje meg, hogy a tömbök, a sor bármelyik eleme elemei közül a szűrő kockára összeállítása és a project az elemeket tetszőleges kombinációját-e azt. 

**Lekérdezés**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Eredmény**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript-integráció
DocumentDB programozási modell nyújt a JavaScript-alapú alkalmazás logikájának végrehajtása közvetlenül a tárolt eljárások és indítók gyűjtemények. Mindkét így:

-   Azt jelenti, hogy a nagy teljesítményű tranzakció alapú CRUD műveletek és a dokumentumokat közvetlenül a adatbázismotor belül JavaScript futtatókörnyezet mély integrációja alapján gyűjtemény lekérdezéseket. 
-   A control flow, változó hatókör meghatározása, és a hozzárendelés és kezelése az adatbázis-tranzakciók primitívek kivétel integrációs természetes modellezési. Ha többet szeretne tudni a JavaScript-integráció DocumentDB támogatása olvassa el a JavaScript server egymás programozási dokumentációját.

###<a name="user-defined-functions-udfs"></a>Felhasználó által definiált függvények (UDF)
Már definiálva, a jelen cikkben felsorolt együtt az DocumentDB SQL támogatja a a felhasználó által definiált függvények (UDF). Skaláris UDF különösen támogatottak, ahol a fejlesztők nulla vagy több argumentum fázisban és eredményezhet egyetlen argumentuma vissza. Minden egyes argumentum ellenőrzi jogi JSON értékek alatt.  

A DocumentDB SQL-szintaxisa terjeszteni a támogatják ezeket a felhasználó által definiált függvények használatával egyéni alkalmazás összefüggés. UDF regisztrálni lehet DocumentDB, és kattintson egy SQL-lekérdezés részeként vonatkozni fog. Erre valójában a UDF exquisitely készült lekérdezések hivatkozhat. Ezt a választást maradhassanak UDF nincs hozzáférése a környezetben objektum, amelyek a JavaScript másfajta (tárolt eljárások és indítók). Lekérdezések írásvédettként végrehajtás, mivel ezek futtatását is lehetővé teszi a elsődleges és másodlagos példányon. Ezért UDF készült eltérően más JavaScript típusú példányon másodlagos futtatásához.

Az alábbi képen egy példa egy UDF hogyan lehet regisztrálni DocumentDB az adatbázist, konkrétan a dokumentum a webhelycsoport csoportban.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Az előző példa létrehoz egy UDF, amelyek neve `REGEX_MATCH`. Elfogadja az JSON megadott két karakterláncérték `input` és `pattern` és ellenőrzi az első találat a második megadott a mintázat JavaScript string.match() függvénnyel.


Most már ábrázolásakor a UDF a kivetítés lekérdezésében. UDF kell minősíteni a kis-és nagybetűket előtaggal "verzióiban." Amikor hívása lekérdezések belül. 

>[AZURE.NOTE] 3/17/2015 előtt DocumentDB támogatott UDF hívások anélkül, hogy a "verzióiban." előtag például REGEX_MATCH() jelölje ki. A hívó mintát elavult.  

**Lekérdezés**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Eredmény**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

Az UDF is használható szűrő belül is minősített a "verzióiban.", az alábbi példában látható módon előtag:

**Lekérdezés**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Eredmény**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


UDF lényegében érvényes skaláris kifejezés, és az előrejelzések és a szűrők használhatók. 

Bontsa ki a UDF az hatványon, tekintse meg egy másik példa feltételes logika a:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Az alábbi képen, hogy az UDF él.

**Lekérdezés**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Eredmény**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Dinamikus az előző példában, mint UDF integrálása a JavaScript nyelv power egy beépített JavaScript futtatókörnyezet funkciók segítségével lépésről, feltételes összetettek elvégzendő gazdag programozható felületet biztosít az DocumentDB SQL.

DocumentDB SQL biztosít az argumentumokat a felhasználói függvényekre mutató minden dokumentum a forrásban szakaszában az aktuális (a WHERE záradék vagy a SELECT záradékban) az UDF feldolgozása. Az eredmény zökkenőmentes beépített a teljes végrehajtás során. Ha a Tulajdonságok említett által az UDF paraméterek nem érhetők el a JSON értéket, a paraméter tekintendő meghatározatlan, és így UDF meghívását teljesen kihagyott. Hasonlóképpen a UDF eredménye nem meghatározott, ha azt nem szerepel az eredményben. 

Az összegzés UDF összetett üzleti logikai funkcióinak a lekérdezés keretében elvégzendő nagyszerű eszköz olyan.

### <a name="operator-evaluation"></a>Kiértékelés operátor
DocumentDB, JSON adatbázisban, alatt rendelkezik áthúzása a JavaScript operátorok és a kiértékelés szemantikáját fekvő. Miközben DocumentDB megpróbálja JavaScript szemantikáját értelmez JSON támogatási megőrzése, a művelet kiértékelés eltér, bizonyos esetekben.

SQL-DocumentDB eltérően hagyományos SQL, az értékek típusú gyakran nem ismert mindaddig, amíg az értékek ténylegesen beolvasása adatbázis. Annak érdekében, hogy hatékony lekérdezéseket, az operátorokat a legtöbb van típus szigorú követelményeknek. 

DocumentDB SQL implicit dokumentumkonvertálás, JavaScript eltérően nem végrehajtása. Például a lekérdezés például `SELECT * FROM Person p WHERE p.Age = 21` megegyezik az életkor tulajdonságot, amelynek az értéke 21 tartalmazó dokumentumok. Bármely más dokumentumot, amelynek kor tulajdonság megegyezik a karakterlánc "21" vagy egyéb esetleg végtelen változatok például "021", "21,0", "0021", "00021", nem fog egyezik a stb. Ez viszont, hogy az adott karakterlánc értékek számok implicit módon casted JavaScript (ex operátor, alapján: ==). Ez a beállítás elengedhetetlen DocumentDB SQL egyező hatékony indexhez. 

## <a name="parameterized-sql-queries"></a>A paraméteres SQL-lekérdezések
DocumentDB lekérdezések támogatja az ismerős kifejezett paraméterekkel @ jelöléssel. Paraméteres SQL biztosít robusztus kezelésére és a felhasználó bemeneti, SQL-beszúrás adatoknak véletlen információk megjelenítése kijutását. 

Például írja be a lekérdezés, amely és a vezetéknevet, valamint a cím állapot paraméterként, és ezután hajtsa végre a különböző értékek és a vezetéknevet, valamint a cím állapot alapján a felhasználó által a.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

A kérés majd lehet küldeni DocumentDB JSON paraméteres lekérdezés például alább látható módon.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Az argumentum, a lap TETEJÉRE beállítható, hogy paraméteres lekérdezésekkel például alább látható módon.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Paraméter értéke lehet bármely érvényes JSON (karakterláncot, a számokat, logikai értékek értéke null, tömbök páros vagy beágyazott JSON). Is mivel DocumentDB séma-kisebb, paramétereket vannak nincs ellenőrizve bármilyen típusú.

##<a name="built-in-functions"></a>A beépített függvények
DocumentDB számos beépített függvények is támogatja a gyakori műveletek, például a felhasználó által definiált függvények (UDF) lekérdezések belül használható.

<table>
<tr>
<td>Matematikai függvények</td> 
<td>ABS, plafon, kitevő, padló, napló, LOG10, POWER, CIKLIKUS, bejelentkezési, gyök, négyszög, csonk, ARCCOS, ARCSIN, ARCTAN, ATN2, COS, akkor a COT, fok, PI, radián, SIN és DRAPP</td>
</tr>
<tr>
<td>Típusellenőrző függvények</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED és IS_PRIMITIVE</td>
</tr>
<tr>
<td>Karakterláncfüggvények</td>   
<td>ÖSSZEFŰZÉS, tartalmaz, ENDSWITH, INDEX_OF, balra, hossza, alsó, LTRIM, csere, REPLIKÁCIÓ, fordított SORREND, jobbra, RTRIM, STARTSWITH, karakterlánc és felső</td>
</tr>
<tr>
<td>Array függvény</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH és ARRAY_SLICE</td>
</tr>
<tr>
<td>Térbeli függvények</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID és ST_ISVALIDDETAILED</td>
</tr>
</table>  

Ha felhőszolgáltatásaiban használt a felhasználó által definiált függvény (UDF), amelynek beépített függvény már érhető el, kell használnia a megfelelő beépített függvény, akkor fog legyen gyorsabban futtatható és hatékonyabban. 

### <a name="mathematical-functions"></a>Matematikai függvények
A matematikai függvények számítást, általában argumentumként szolgálnak, és a számértéket ad vissza bemeneti értékek alapján. Az alábbiakban táblázatos támogatott beépített matematikai függvények.

<table>
<tr>
<td><strong>Használat</strong></td>
<td><strong>Leírás</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>A megadott numerikus kifejezés (pozitív) abszolút értékét adja eredményül.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">PLAFON (num_expr)</a></td> 
<td>A legkisebb egész szám értékét adja eredményül, nagyobb vagy egyenlő, a megadott numerikus kifejezés.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">Padló (num_expr)</a></td> 
<td>Adja eredményül a legnagyobb egész kisebb vagy egyenlő a megadott numerikus kifejezés.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Adja vissza a kitevő a megadott numerikus kifejezés.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (num_expr [, Alap])</a></td> 
<td>Adja eredményül a megadott numerikus kifejezés vagy a használja az adott számrendszerben megadott alapú logaritmusát a természetes logaritmusa</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>A 10-es logaritmikus a megadott numerikus kifejezés értékét adja eredményül.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">KEREKÍTÉS (num_expr)</a></td> 
<td>Adja eredményül egy numerikus érték, a legközelebbi egész számra kerekíti.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">CSONK (num_expr)</a></td> 
<td>Adja eredményül egy numerikus érték, a program csak a legközelebbi egész számra kerekíti.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">GYÖK (num_expr)</a></td>   
<td>A megadott numerikus kifejezés négyzetgyökét adja eredményül.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">NÉGYZETES (num_expr)</a></td>   
<td>Kiszámítja a megadott numerikus kifejezés.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">HATVÁNY (num_expr; num_expr)</a></td>   
<td>A power a megadott numerikus kifejezés az érték a megadott adja vissza.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">BEJELENTKEZÉSI (num_expr)</a></td>   
<td>A megadott numerikus kifejezés bejelentkezési értékét (-1, 0; 1) adja eredményül.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ARCCOS (num_expr)</a></td>   
<td>A szöget radiánban, amelynek a koszinusza a megadott numerikus kifejezés; adja eredményül. arkusz néven is ismert.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ARCSIN (num_expr)</a></td>   
<td>Szöget radiánban, amelynek szinusza a megadott numerikus kifejezés adja eredményül. Arkusz ezt is nevezik.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ARCTAN (num_expr)</a></td>   
<td>Szöget radiánban, amelynek tangense a megadott numerikus kifejezés adja eredményül. Arkusz ezt is nevezik.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Szöget adja eredményül, radiánban a között pozitív x tengely és a ray a eredetű (y, x), a pontra, ahol x és y az értékeket a megadott lebegtetés két kifejezésből.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>A megadott szög trigonometriai koszinuszát adja eredményül a megadott kifejezés radiánban.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>A megadott szög trigonometriai kotangensét adja eredményül a megadott numerikus kifejezés radiánban.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">FOK (num_expr)</a></td> 
<td>Az egy radiánban megadott szög fokban megfelelő szöget adja eredményül.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">A PI)</a></td>   
<td>A pi matematikai állandó értékét adja eredményül.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIÁN (num_expr)</a></td> 
<td>Radián adja vissza, egy numerikus kifejezés-fokban, beírásakor.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>A megadott szög trigonometriai szinuszát adja eredményül a megadott kifejezés radiánban.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>A beviteli kifejezés tangensét adja eredményül a megadott kifejezésben.</td>
</tr>

</table> 

Például most futtathatók lekérdezések, például a következőket:

**Lekérdezés**

    SELECT VALUE ABS(-4)

**Eredmény**

    [4]

Fő közötti DocumentDB a függvények és az ANSI SQL összehasonlítása különbség, hogy azok jól séma-kisebb és a vegyes séma adatokkal végzett munkához készültek. Ha például egy dokumentumot, amennyiben a méret tulajdonság hiányzik, vagy ha egy nem numerikus érték, mint "Ismeretlen", majd a dokumentum helyett a hibát visszaadó kimarad fölé.

### <a name="type-checking-functions"></a>Típusellenőrző függvények
A típus ellenőrzési függvény lehetővé teszi az SQL-lekérdezések belül kifejezés típusának ellenőrzése. Ellenőrző függvények típus menet dokumentumokból tulajdonságok típusú határozza meg, amikor változó vagy ismeretlen használható. Az alábbiakban a támogatott beépített típusellenőrző függvények tartalmazó táblázat.

<table>
<tr>
  <td><strong>Használat</strong></td>
  <td><strong>Leírás</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (kifejezés)</a></td>
  <td>Olyan logikai érték, amely azt jelzi, ha az érték típusától tömb lekérdezése</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (kifejezés)</a></td>
  <td>Adja eredményül, ami azt jelzi, ha az érték típusától logikai érték logikai érték.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (kifejezés)</a></td>
  <td>Olyan logikai érték, amely azt jelzi, ha az érték típusától null lekérdezése</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (kifejezés)</a></td>
  <td>Olyan logikai érték, ami azt jelzi, ha az érték típusától egy számot ad eredményül</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (kifejezés)</a></td>
  <td>Olyan logikai érték, ami azt jelzi, ha az érték típusától egy JSON-objektum lekérdezése</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (kifejezés)</a></td>
  <td>Olyan logikai érték, ami azt jelzi, ha az érték típusától karakterlánc lekérdezése</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (kifejezés)</a></td>
  <td>Jelzi, ha a tulajdonság van hozzárendelve egy érték logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (kifejezés)</a></td>
  <td>Olyan logikai érték, amely azt jelzi, ha az érték típusától karaktersorozat, szám, logikai érték vagy null lekérdezése</td>
</tr>

</table>

Használja ezeket a funkciókat, most futtathatók lekérdezések, például a következőket:

**Lekérdezés**

    SELECT VALUE IS_NUMBER(-4)

**Eredmény**

    [true]

### <a name="string-functions"></a>Karakterláncfüggvények
A következő skaláris függvények bemeneti szöveges értékként a művelet végrehajtása, és vissza egy karakterlánc, numerikus vagy logikai érték. A következő táblázat a beépített karakterláncfüggvények:

Használat|Leírás
---|---
[HOSSZA (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|A megadott karakterláncot kifejezés karakterek számát adja meg.
[ÖSSZEFŰZÉS (str_expr str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Két vagy több megadott értékeket kifejezések eredménye karakterláncot ad eredményül.
[KARAKTERLÁNC (str_expr, num_expr num_expr.)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Egy karakterlánc részét adja eredményül.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Egy logikai jelző eredménye e az első karakterlánc a második végződése
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Egy logikai jelző eredménye akár az első karakterlánc a második végződése
[CONTAINS (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Egy logikai jelző függvény a második hogy az első karakterlánc tartalmazza.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|A második első előfordulásának kezdő pozícióját adja vissza karakterlánc belül az adott karakterlánc első kifejezést, vagy a -1, ha a karakterlánc nem található.
[LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|A megadott számú karaktert tartalmazó karakterlánc bal részét adja eredményül.
[RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|A megadott számú karaktert tartalmazó karakterlánc jobb részét adja eredményül.
[LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Egy karakterlánc adja eredményül, után az első üres eltávolítja.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Egy karakterlánc összes záró szóközöket csonkítása után lekérdezése
[ALSÓ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Adja eredményül nagybetűs karakter adatok átalakítása kisbetűssé után egy karakterlánc.
[FELSŐ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Karakterlánc-kifejezés, nagybetű karakter adatok átalakítása nagybetűssé után eredménye.
[Csere (str_expr, str_expr str_expr.)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Egy adott karakterlánc összes előfordulását egy másik karakterlánc értékre cseréli.
[A REPLIKÁCIÓ (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Egy karakterlánc megadott számú alkalommal megismétlése.
[Fordított SORREND (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|A fordított sorrendben, a karakterlánc értéket ad eredményül

Ezek a függvények használata esetén most futtathatók lekérdezések az alábbihoz hasonló. Ha például bármikor visszatérhet a Vezetéknév nagybetűs az alábbi képlettel történik:

**Lekérdezés**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Eredmény**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Vagy összefűzés, például ebben a példában:

**Lekérdezés**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Eredmény**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Karakterláncfüggvények is használható a WHERE záradékban szűrést, például az alábbi példában:

**Lekérdezés**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Eredmény**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Array függvény
A következő skaláris függvények végrehajtása egy tömb bemeneti értékek és numerikus visszatérési, logikai érték vagy tömbben értéket. Az alábbiakban táblázatos beépített tömb függvények:

Használat|Leírás
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|A megadott tömb kifejezés elemek számát adja vissza.
[ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Két vagy több tömb értékeket kifejezések eredménye egy tömböt adja eredményül.
[ARRAY_CONTAINS (arr_expr, kifejezés)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Jelzi, hogy a tömb tartalmazza a megadott érték logikai érték beolvasása.
[ARRAY_SLICE (arr_expr num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Egy tömb kifejezés részét adja eredményül.

Array függvény tömbök belül JSON módosítására használható. Íme például egy lekérdezés, amely visszaadja a minden dokumentum hol található a szülők közül a "Robin Wakefield". 

**Lekérdezés**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Eredmény**

    [{
      "id": "WakefieldFamily"
    }]

Íme egy másik példa, amelyből ARRAY_LENGTH egy család gyermekek számát.

**Lekérdezés**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Eredmény**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Térbeli függvények

DocumentDB támogatja a következő nyissa meg a földrajzi Consortium (OGC) a beépített függvények térinformatikai lekérdezésére. Térinformatikai rendszeren DocumentDB támogatást, olvassa el a [Azure DocumentDB az eszközzel térinformatikai adatokat vizsgálhat használata](documentdb-geospatial.md)című témakört. 

<table>
<tr>
  <td><strong>Használat</strong></td>
  <td><strong>Leírás</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>A két GeoJSON pont kifejezések közé eső a távolság.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Adja eredményül, jelezve, hogy a GeoJSON pont, az első argumentumban megadott érték belül a GeoJSON sokszög a második argumentumban szereplő logikai kifejezés.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Jelzi, hogy a megadott GeoJSON pont, illetve sokszög kifejezés érvényes logikai értéket ad eredményül.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>JSON érték logikai érték tartalmazó megadott GeoJSON pont, illetve sokszög kifejezés érvényes, és ha érvénytelen értéket adja eredményül, ezenkívül egy karakterláncértéket okát.</td>
</tr>
</table>

Térbeli függvények ellen térbeli adatok közelében querries végrehajtásához használható. Ha például az alábbiakban az összes családi dokumentumokat, amelyek a megadott helyen, a ST_DISTANCE beépített függvénnyel 30 km belül visszaadó lekérdezés. 

**Lekérdezés**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Eredmény**

    [{
      "id": "WakefieldFamily"
    }]

Ha az indexelési házirend térbeli indexelés, majd "távolság lekérdezések" szolgáltató hatékony az index keresztül. A térbeli indexelés, kérjük, lásd: az alábbi szakaszt. Ha még nincs telepítve a megadott elérési útja térbeli index létrehozása, akkor is elvégezhetők térbeli lekérdezések megadásával `x-ms-documentdb-query-enable-scan` kérelem fejlécben értéke "igaz". A .NET ez történik a választható **FeedOptions** argumentum [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) igaz értékre kell állítani a lekérdezések megkerülhetők. 

Ha valamit esik sokszög ellenőrzése ST_WITHIN használható. Sokszög gyakran az irányítószámokat, állapot határai és természetes alakzatokba határai jelölésére használhatók. Ismét Ha térbeli indexelés az indexelési házirendet, majd lekérdezések "belül" szolgáltató hatékony az index keresztül. 

ST_WITHIN sokszög argumentumok csak csengetés is tartalmazhat, azaz a sokszög holes bennük nem tartalmazhat. Jelölje be a [DocumentDB korlátai](documentdb-limits.md) pontok ST_WITHIN lekérdezés sokszög rajzolása a megengedett maximális száma.

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Eredmény**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Hogyan egyező típusú működik, de DocumentDB lekérdezés, ha a hely értéket a megadott, vagy argumentum hibás vagy érvénytelen, akkor azt **nem meghatározott** és a lekérdezés eredményéből kihagyja kiértékelt dokumentum értékeli hasonló. Ha a lekérdezés eredménye nem tartalmaz, futtassa a ST_ISVALIDDETAILED szeretné hibakeresése miért spatail típusa érvénytelen.     

Annak ellenőrzése, ha a térbeli objektum érvényes ST_ISVALID és ST_ISVALIDDETAILED használható. Az alábbi lekérdezés ellenőrzi, például a "Házon kívül" tartomány szélesség értékét (-132.8) pont érvényességét. ST_ISVALID olyan logikai értéket adja eredményül, és ST_ISVALIDDETAILED ad eredményül, logikai és az OK, miért célszerű érvénytelen tartalmazó karakterlánc.

**Lekérdezés**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Eredmény**

    [{
      "$1": false
    }]

Az előbbi függvényekkel is használható sokszög érvényesítéséhez. Ha például az alábbi használjuk ST_ISVALIDDETAILED érvényesítésére nem bezárt sokszög rajzolása. 

**Lekérdezés**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Eredmény**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Fussa térbeli függvények és az SQL-szintaxisa, az DocumentDB. Most már most tekintsünk át egy hogyan dolgozik, és hogy, hogyan kommunikáljon a az alábbi lekérdezés LINQ azt láthatta, az eddigi.

## <a name="linq-to-documentdb-sql"></a>DocumentDB SQL LINQ
LINQ .NET programozási modell, amely kiszámítása az objektumok adatfolyam megjelenítését a lekérdezésként kifejező. DocumentDB nyújt az ügyfél által JSON és a .NET-objektumok és LINQ egy részét a megfeleltetés közötti konverzió megkönnyítése LINQ felülettel egymás tár lekérdezések DocumentDB lekérdezések. 

Az alábbi képen LINQ lekérdezések DocumentDB támogató architektúráját.  A DocumentDB ügyfelet használ, a fejlesztők, majd a LINQ lekérdezés fordítja DocumentDB lekérdezés DocumentDB lekérdezés szolgáltató közvetlenül lekérdezések **IQueryable** objektum hozhat létre. A lekérdezés majd átadott egy sor olyan JSON formátumban eredmények beolvasásához DocumentDB kiszolgálóra. A visszaadott találatok .NET-objektumok ügyféloldali adatfolyam be vannak deszerializálni.

![Architektúra igazoló LINQ lekérdezések DocumentDB - SQL-szintaxisa, JSON-lekérdezési nyelv, adatbázis fogalmak és SQL-lekérdezések][1]
 


### <a name="net-and-json-mapping"></a>.NET és a JSON-megfeleltetés
.NET-objektumok és JSON dokumentumok közötti hozzárendelést természetes - minden tag adatmezőt hozzá van rendelve egy JSON-objektum, ahol a mező nevét a "billentyűt" rész az objektum van hozzárendelve és a "értéket" rész rekurzív megfeleltetve az objektum érték részét. Vegye figyelembe az alábbi példa. A létrehozott család objektum van hozzárendelve a JSON-dokumentumot, alább látható módon. És ez fordítva is igaz, a JSON-dokumentum van rendelve vissza egy .NET-objektumot.

**C# osztály**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>Az SQL-fordítási LINQ
A DocumentDB lekérdezés szolgáltató hajt végre, egy DocumentDB SQL-lekérdezés átalakítása legjobb munkamennyiség megfeleltetés LINQ lekérdezésből. A következőhöz hasonló leírás feltételezzük az olvasót van egy egyszerű ismertségét LINQ.

Első lépésként a típus rendszer támogatjuk JSON egyszerű diagramtípusokat – numerikus típusú, a logikai érték, a karakterlánc és a null. Ezek a JSON típusok támogatottak. A következő skaláris kifejezés támogatottak.

-   Állandók – ezek állandó értékeket tartalmaz a egyszerű adattípusú kiértékeli a lekérdezés időben.

-   Tulajdonság/tömb index kifejezések – ezeket a kifejezéseket olvassa el az objektum vagy egy tömb elem tulajdonsága.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Számtani kifejezések – ezek közé tartozik a numerikus és logikai értékeket a gyakori aritmetikai kifejezéseket. A teljes listát olvassa el az SQL-beállítások.

        2 * family.children[0].grade;
        x + y;

-   Karakterlánc-összehasonlítás – ezek közé tartozik néhány állandó karakterlánc értékét szöveges értékként összehasonlítása.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Objektum/tömb létrehozása kifejezés - e kifejezések visszatérési összetett értéket vagy névtelen típusú objektum vagy ilyen objektumok tömbjét. Ezek az értékek egymásba.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Támogatott LINQ operátorok listája
Az alábbiakban a LINQ szolgáltatóban az DocumentDB .NET SDK részét képező támogatott LINQ operátorok listáját.

-   **Válassza a**: az SQL, jelölje be köztük objektum építés fordítása előrejelzések
-   **Hol**: szűrők Célnyelv listájában a SQL WHERE, ez utóbbiak pedig közötti fordítás & &, || és! az SQL-szereplők
-   **SelectMany**: lehetővé teszi, hogy a Csatlakozás SQL-záradék a tömbök visszagörgetésének. Használható lánc/fészekből tömbelemek szűrni kifejezések
-   **OrderBy és OrderByDescending**: megfelelője RENDEZÉS növekvő vagy csökkenő lehet:
-   **CompareTo**: tartomány összehasonlításokat megfelelője. Gyakran használt karakterláncok, mivel azok nem a .NET hasonló
-   **Vegyék**: megfelelője SQL felső korlátozza a lekérdezés eredménye
-   **Matematikai függvények**: a fordítás támogatja. A nettó Abs, ARCCOS, ARCSIN, ARCTAN, Cos felső határa, kitevő, padló, naplófájl, Log10, Pow, ciklikus, bejelentkezési, Sin, gyök, Tan, az egyenértékű SQL beépített funkciók Truncate.
-   **Karakterláncfüggvények**: a fordítás támogatja. NETTÓ 's összefűzés, tartalmaz, EndsWith, IndexOf, darab, ToLower, TrimStart, csere, fordított sorrend, TrimEnd, StartsWith, karakterlánc, az egyenértékű SQL beépített funkciók ToUpper.
-   **Tömb funkciók**: a fordítás támogatja. A nettó összefűzés, tartalmazza, és száma lehetőséget választva az egyenértékű a beépített SQL-függvényekkel.
-   **Földrajzi bővítmény funkciók**: a megfelelő SQL beépített funkciók helyettes módszer áll rendelkezésére távolság belül IsValid és IsValidDetailed fordítási támogatja.
-   **Felhasználó által definiált függvény bővítmény függvény**: támogatja a fordítási helyettes metódusa UserDefinedFunctionProvider.Invoke a megfelelő felhasználó által definiált függvény.
-   **Egyéb**: egyesítés és feltételes operátorok fordítási támogatja. Lefordíthatja a karakterlánc tartalmaz, a ARRAY_CONTAINS vagy a környezettől függően az SQL-IN Contains.

### <a name="sql-query-operators"></a>SQL-lekérdezés operátorok
Az alábbiakban néhány példát talál, amelyek bemutatják, hogyan lefordított egyes szabványos LINQ lekérdezés operátorok az DocumentDB lekérdezések le.

#### <a name="select-operator"></a>Jelölje ki a operátor
Szintaxisa a következő `input.Select(x => f(x))`, ahol `f` skaláris kifejezés.

**LINQ lambda kifejezés**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda kifejezés**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda kifejezés**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operátor
Szintaxisa a következő `input.SelectMany(x => f(x))`, ahol `f` egy skaláris kifejezés, amely visszaadja a webhelycsoport típus van.

**LINQ lambda kifejezés**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Ha operátor
Szintaxisa a következő `input.Where(x => f(x))`, ahol `f` van egy skaláris kifejezés, amely egy logikai értéket ad eredményül.

**LINQ lambda kifejezés**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda kifejezés**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Összetett SQL lekérdezésekkel
Lehet, hogy a fenti operátorok hatékonyabb lekérdezések kialakításához tevődik össze. DocumentDB támogatja a beágyazott gyűjtemények, mivel a kialakítási kell tömb összefűzéséből vagy beágyazott.

#### <a name="concatenation"></a>Összefűzés 

Szintaxisa a következő `input(.|.SelectMany())(.Select()|.Where())*`. Egybeírt lekérdezés kiindulhat egy választható `SelectMany` lekérdezés követ több `Select` vagy `Where` operátorok.


**LINQ lambda kifejezés**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda kifejezés**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda kifejezés**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda kifejezés**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Beágyazása

Szintaxisa a következő `input.SelectMany(x=>x.Q())` hol található a Q egy `Select`, `SelectMany`, vagy `Where` operátor.

Beágyazott lekérdezés a belső lekérdezés minden elemet a külső gyűjtemény érvényes. Egy fontos funkció, hogy a belső lekérdezés megvalósítását célul kitűző fejlesztők a mezőket az elemek, például a külső gyűjteményben Önillesztések.

**LINQ lambda kifejezés**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda kifejezés**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda kifejezés**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>SQL-lekérdezések végrehajtása
DocumentDB közzététele erőforrásokat a REST API-t, bármelyik alkalmas az, hogy a HTTP-/ HTTPS-kérések nyelv hívható keresztül. Emellett a DocumentDB felajánlja a különféle programozási tárakat, például a .NET rendszerhez, a Node.js, a JavaScript és Python számos népszerű nyelvhez. A REST API-val és a különböző tárak összes támogatja a lekérdezés SQL keresztül. A .NET SDK lekérdezés SQL kívül LINQ támogatja.

Az alábbi példák bemutatják, hogyan hozhat létre lekérdezés, és küldje el egy DocumentDB adatbázis-fiók.

### <a name="rest-api"></a>REST API-VAL
DocumentDB a HTTP-megnyitott RESTful programozási modellt biztosít. Adatbázis-fiókokat az Azure-előfizetést használ kell építenie. A DocumentDB erőforrás modellben egy csoportja, amelyek címmel rendelkező logikai és állandó URI használata az adatbázis-fiókot forrásokra. Erőforrások csoportja a dokumentum hírcsatorna nevezik. Egy adatbázis-fiókot áll egy sor olyan, adatbázisokat több gyűjtemények, ahol minden egyes melyik az-turn tartalmazó dokumentumok, függvényekben és más erőforrás típusú.

Az egyszerű kapcsolati modell, az alábbi forrásokból lesz a HTTP-műveletek keresztül kérése, a helyezése, a bejegyzés és a Törlés a szabványos értelmezését. A bejegyzés igei egy új erőforrás elrejtésével, a tárolt eljárás végrehajtása, illetve DocumentDB lekérdezés kiadására használják. Lekérdezések mindig olvasnak csak a nem egymás effektusok műveleteket.

Az alábbi példák bemutatják egy BEJEGYZÉSBEN DocumentDB lekérdezés a két minta dokumentumokat tartalmazó gyűjtemény ellen azt az eddigi áttekintése. A lekérdezés a JSON name tulajdonság egy egyszerű szűrő van. Megjegyzés: a használatát a `x-ms-documentdb-isquery` és a tartalomtípus-: `application/query+json` fejlécek, hogy a művelet-e a lekérdezés jelölésére.


**Kérés**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Eredmény**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


A második példa bemutatja, hogy több értéket ad vissza az illesztés összetettebb lekérdezés.

**Kérés**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Eredmény**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Ha a lekérdezés eredményében nem fér belül az eredmények egy oldalra, majd a REST API-t egy folytatását jelző token keresztül adja eredményül a `x-ms-continuation-token` válasz fejléc. Ügyfelek által a fejlécekkel együtt az azt követő között is megjelenítheti az eredmények. Találatok oldalankénti száma keresztül is szabályozhatja a `x-ms-max-item-count` szám fejléc.

Az adatok konzisztencia házirendet a lekérdezések kezeléséhez használja a `x-ms-consistency-level` fejléc, például az összes REST API-kérés. Munkamenet konzisztencia, a legújabb is echo szükség a `x-ms-session-token` a lekérdezési kérelem cookie-fejléc. Figyelje meg, hogy az indexelő a lekérdezett webhelycsoport-házirend is befolyásolhatja a lekérdezés eredményeinek következetességét. Az az alapértelmezett házirend-beállítások az indexelés, gyűjtemények az index mindig a dokumentum tartalmát az aktuális és lekérdezési eredmények fognak egyezni a kiválasztott adatok következetességét. Az indexelési házirend Lusta van enyhíteni, ha lekérdezések elavult eredményt is vissza. További tudnivalókért tekintse át a [DocumentDB konzisztencia szintek] [consistency-levels].

Ha a webhelycsoport a beállított indexelési házirend nem támogatja a megadott lekérdezés, a a DocumentDB kiszolgáló 400 "Hibás kérés" adja eredményül. Ez a függvény az elérési út az keresések kivonat (egyenlő), és a görbék indexelésből explicit módon beállított tartomány lekérdezéseket ad vissza. A `x-ms-documentdb-query-enable-scan` élőfej engedélyezése a keresés végrehajtását, ha az index nem érhető el a lekérdezés adható meg.

### <a name="c-net-sdk"></a>C# (.NET) SDK
A .NET SDK támogatja LINQ és az SQL lekérdezésére. A következő példa bemutatja, hogyan végezhetők el az egyszerű szűrő lekérdezés, a dokumentum korábbi jelent meg.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Ez a példa két egyenlő belül minden dokumentum tulajdonságainak hasonlítja össze, és használja a névtelen előrejelzések. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


A következő példa bemutatja a illesztések, LINQ SelectMany keresztül kifejezni.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



A .NET-ügyfél automatikusan függvény között a lekérdezés eredményében a foreach blokkokban, mint a fenti összes oldalát. A lekérdezés beállításai a REST API-szakaszban jelent meg is megtalálhatók a .NET SDK használata a `FeedOptions` és `FeedResponse` CreateDocumentQuery módszer az osztályok. Az oldalak számát vezérelhető a `MaxItemCount` beállítást. 

Közvetlenül is megadhatja, hogy lapozási létrehozásával `IDocumentQueryable` használata a `IQueryable` objektumra, majd olvasásával a` ResponseContinuationToken` értékeket, és haladnak őket biztonsági másolatot, mint `RequestContinuationToken` a `FeedOptions`. `EnableScanInQuery`Keresés engedélyezése, ha a lekérdezés által az indexelési beállított házirend nem támogatott adhatja meg. Particionált gyűjtemények, használhatja `PartitionKey` egyetlen szemben a lekérdezés futtatásához partition (bár DocumentDB is automatikusan kinyerése ezt a lekérdezés szövegét), és `EnableCrossPartitionQuery` ellen több partíciót futtatásához szükséges lekérdezések futtatásához. 

Olvassa el az [DocumentDB.NET](https://github.com/Azure/azure-documentdb-net) mintákra tartalmazó lekérdezések további minták. 

### <a name="javascript-server-side-api"></a>A JavaScript kiszolgálóoldali API 
DocumentDB programozási modell nyújt a JavaScript-alapú alkalmazás logikájának végrehajtása közvetlenül a tárolt eljárásokkal és indítók segítségével gyűjtemények. A JavaScript logikájának regisztrált a webhelycsoport szintjén az adott webhelycsoport működésének a dokumentumokon az adatbázis-műveletek majd bocsát ki. Ezek a műveletek vannak csomagolni környezeti savas tranzakciók.

Az alábbi példa a queryDocuments használatáról JavaScript API Server, hogy a lekérdezések megjelenítése belső tárolt eljárások és indítók.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Összesítő függvények

Összesítő függvények natív támogatása az együttműködik, de ha időközben darabszám vagy a SZUM funkciót, érhet el különböző módszerekkel ugyanazt az eredményt.  

A olvasási elérési útja:

- Adatok beolvasása és helyileg száma módon összesítő függvények végezheti el. Azt van, azt javasoljuk, például egy olcsó lekérdezés kivetítés használata `SELECT VALUE 1` teljes dokumentum olyan, nem pedig annak `SELECT * FROM c`. Ezzel az elemzéssel maximalizálása az eredményeket, ezáltal, amellyel elkerülheti a szolgáltatás állapotát további a körbejárásokhoz szükség esetén minden oldalára feldolgozott dokumentumok száma.
- A tárolt eljárás használatával összezárása ismételt ciklikus utakat a hálózati késés. Tárolt eljárás, amely kiszámítja az egy adott szűrő lekérdezés száma a minta [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js)témakörben talál. A tárolt eljárás engedélyezheti a felhasználók egyesítéséhez gazdag logikájának összesítések módon hatékony módon együtt.

A írási elérési útja:

- Egy másik közös mintát is előre összesítéséhez az "írási" elérési út az eredményeket. Ez akkor különösen vonzó, ha a "olvassa el" kérések hangereje nagyobb, mint az "írja be" összehívások. Egyszer előre összesített, az olvasási visszaigazolás kérése pont az eredmények állnak rendelkezésre.  A legjobb módszer a DocumentDB előre összesítése, amely az egyes "írási" meghívott eseményindító beállítása és frissítése a metaadat-dokumentum, amely tartalmazza a lekérdezést, amely az éppen eseményre legújabb eredményeit. Például kérjük, tekintse meg a [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) minta frissíti a minSize maxSize és a metaadat-alapú dokumentum a webhelycsoport totalSize. A minta kiterjeszthetők frissítése egy számláló, SZUM stb.

##<a name="references"></a>Hivatkozások
1.  [Azure DocumentDB – bevezetés][introduction]
2.  [DocumentDB SQL meghatározása](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [DocumentDB .NET minták](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB konzisztencia szintek][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  A JavaScript specifikációja [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  A lekérdezés nagy adatbázisok [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611) kiértékelés módszerei
10. A párhuzamos relációs adatbázisból rendszerek, nyomja le a számítógép IEEE társasága, feldolgozása 1994 lekérdezés
11. Lu, Ooi, Tan, a lekérdezések 1994 párhuzamos relációs adatbázisból rendszerek, nyomja le a számítógép IEEE társasága, a feldolgozása.
12. Christopher Olston, Benjamin Reed Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: malac Latin: A nem úgy idegen nyelvi adatkezelési SIGMOD 2008.
13.     AZOK Graefe. A lekérdezés optimalizálási kaszkádokban kerete. IEEE adatok Eng. BULL., 18(3): 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
