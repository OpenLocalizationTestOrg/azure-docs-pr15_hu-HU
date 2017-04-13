<properties 
    pageTitle="Hivatkozás az alkalmazás az összefüggéseket Analytics |} Microsoft Azure" 
    description="Összefoglalás: a kimutatások Analytics, az alkalmazás az összefüggéseket a hatékony keresés eszköz. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Hivatkozás elemzéséhez

[Analytics](app-insights-analytics.md) a hatékony keresés funkció az [Alkalmazás az összefüggéseket](app-insights-overview.md). Ezeket a lapokat a Analytics-lekérdezési nyelv ismertetik.

> [AZURE.NOTE] [Tesztelése Analytics, hogy szimulált adatait a](https://analytics.applicationinsights.io/demo) Ha az alkalmazás nem küld adatokat alkalmazás mélyebb még.

## <a name="index"></a>Index


**Engedélyezése** [engedélyezése](#let-clause)


**Lekérdezések és műveleti jelek** [Darabszám](#count-operator)  |  [felmérése](#evaluate-operator) | [kiterjesztése](#extend-operator) | [join](#join-operator) | [korlát](#limit-operator) | [mvexpand](#mvexpand-operator) | [elemezni](#parse-operator) | [project](#project-operator) | [project-nem vagyok a gépnél](#project-away-operator) | [tartomány](#range-operator) | [csökkentése](#reduce-operator) | [irányelv jeleníti meg](#render-directive) | [záradék korlátozása](#restrict-clause) | [Rendezés](#sort-operator) | [Összegzés](#summarize-operator) | [készítése](#take-operator) | [felső](#top-operator) | [felső egymásba ágyazott](#top-nested-operator) | [Unió](#union-operator) | [Hol](#where-operator) | [where a](#where-in-operator)

**Összesítések** [bármely](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [darab](#count) | [DARABTELI](#countif) | [AB.darab](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [PERCENTILIS](#percentile) | [percentilisek](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [szórás](#stdev) | [SZUM](#sum) | [eltérés](#variance)

**Scalars** [Logikai literálok](#boolean-literals)  |  [Logikai operátorokat](#boolean-operators) | [kiterjesztések](#casts) | [skaláris összehasonlításokat](#scalar-comparisons) | [gettype](#gettype) | [kivonat](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Számok** [Számtani operátorok](#arithmetic-operators)  |  [Numerikus literálok](#numeric-literals) | [abs](#abs) | [bin](#bin) | [kitevő](#exp) | [padló](#floor) | [gamma](#gamma) | [log](#log) | [VÉL](#rand) | [gyök](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Dátum és idő** [Dátum és idő kifejezések](#date-and-time-expressions)  |  [Dátum és idő literálok](#date-and-time-literals) | [korábbi](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [most](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Karakterlánc** [GUID](#guids)  |  [Szövegkonstanssal kódolták](#obfuscated-string-literals) | [Szövegkonstanssal](#string-literals) | [karakterláncok összehasonlításakor](#string-comparisons) | [countof](#countof) | [kibontása](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [cseréje](#replace) | [felosztása](#split) | [strcat](#strcat) | [strlen](#strlen) | [karakterlánc](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Tömbök, objektumok és dinamikus** [Tömb és objektum literálok](#array-and-object-literals)  |  [Dinamikus objektum függvények](#dynamic-object-functions) | [küldő záradékok dinamikus objektumainak](#dynamic-objects-in-let-clauses) | [JSON elérési út kifejezések](#json-path-expressions) | [nevek](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [tartomány](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Engedélyezése

### <a name="let-clause"></a>tudathatja záradék

**Táblázatos lehetővé teszik - táblázat elnevezése**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skaláris lehetővé teszik - kiosztási érték**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda lehetővé teszik - kiosztási függvény**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

A küldő záradék [neve](#names) kötődik táblázatos eredmény, skaláris értéket vagy függvény. A záradék egy előtagot, amelyet egy lekérdezést, és a kötést hatóköre, amelyek lekérdezik. (Hogy nem kínál a munkamenet később használt név elemeinek.)

**Szintaxis**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *típusa:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* A lekérdezés nem alapértelmezés szerint egy legyen záradék.

**Példák**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Saját illesztés:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Lekérdezések és műveleti jelek

A telemetriai egy lekérdezése szűri egy folyamat követ forrás adatfolyam hivatkozás tevődik össze. Példa:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Minden szűrő a függőleges vonal karakter elé `|` *operátort*, néhány paraméter egy példánya. A beviteli az operátor a táblázatot, amelyet az előző folyamat eredménye. A legtöbb esetben a paramétereket keresztül az oszlopokat a bevitt történik [skaláris kifejezés](#scalars) . Néhány esetben a paraméterek azoknak a bemeneti oszlopot, és néhány esetben a paraméter egy második tábla. A lekérdezés eredménye mindig egy táblázatban, még akkor sem ha csak egy oszlop- és egy.

Lekérdezések egyetlen sortörések is tartalmazhat, de megszakítja egy üres sort. A megjegyzések közötti tartalmazhatnak `//` és a sor végére.

Lekérdezés is kell alapértelmezés szerint egy vagy több [clauses engedélyezni](#let-clause), amelyek meghatározzák, hogy scalars, táblázatot vagy a lekérdezésen belül használható függvények.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`használják az alábbi lekérdezés például az előző folyamat vagy a forráslistában táblázatban jelölésére.
> 

### <a name="count-operator"></a>darab operátor

A `count` operátor rekordokat (sorokat) számát adja meg a bemeneti rekordkészletben.

**Szintaxis**

    T | count

**Argumentumok**

* *Kétmintás T*: A táblázatos adatok, amelynek rekordjait megszámolni vannak.

**Adja eredményül.**

Ez a funkció egy egyetlen olyan rekordot és típusú oszlop egy táblázatot ad vissza, `long`. A *T*a rekordok számát a egyetlen cella értéke. 

**Példa**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>operátor felmérése

`evaluate`egy bővítmény mechanizmusa, amely lehetővé teszi speciális algoritmusok lekérdezések beszúrandó van.

`evaluate`a lekérdezés során az utolsó üzemeltető kell lennie (kivéve a lehető `render`). Meg kell nem jelennek meg a függvény törzsébe.

[autocluster felmérése](#evaluate-autocluster) | [kosár felmérése](#evaluate-basket) | [diffpatterns felmérése](#evaluate-diffpatterns) | [extractcolumns felmérése](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>autocluster felmérése

     T | evaluate autocluster()

AutoCluster különálló attribútumok (méret) közös mintázatok megkeresi az adatokat, és mintázatok kis számú csökkenti a (hogy-e 100 vagy 100 KB sorok), az eredeti lekérdezés eredményét. AutoCluster segíti a hibák (például a kivételek, lefagy a) elemzése kifejlesztett, de esetleg bármely szűrt adatkészlet is dolgozhatnak. 

**Szintaxis**

    T | evaluate autocluster( arguments )

**Adja eredményül.**

AutoCluster (általában kicsi) a lásson mintákat, amelyek az adatok megosztott közös értékű részeire rögzítése több különálló attribútumok keresztül. Minden minta az eredményeket egy-egy sor jelöli. 

Az első két oszlop, a darab és a sorok ki az eredeti lekérdezés által a mintázat rögzítendő százalékát. Az eredeti lekérdezésből a többi oszlop és értékük, az oszlop vagy egy adott érték vagy "*" változóértékkel azaz. 

Figyelje meg, hogy a forgalmat ne legyenek különálló: Előfordulhat, hogy átfedik egymást, és általában nem foglalkozik az eredeti sorokat. Bizonyos sorokat nem tartozhatnak bármely minta területen.

**Tippek**

* Használat `where` és `project` be a bemeneti függőleges vonás csökkentheti az adatokat, hogy éppen mi, amely érdekli.
* Amikor talál egy érdekes sor, érdemes lehet az adott értékeit való hozzáadásával tovább lehatolhatnak a `where` szűrő.

**Az argumentumok (az összes nem kötelező)**

* `output=all | values | minimal` 

    Az eredmények formátum. A darab és a készültségi mindig megjelennek az eredmények. 

 * `all`-a bemeneti az összes oszlop kimeneti
 * `values`-oszlopok csak kiszűri "*" a találatok között
 * `minimal`-is kiszűri oszlopokat, amelyeket azonos-e az eredeti lekérdezés összes sorát. 


* `min_percent=`*dupla* (alapértelmezett: 1)

    A létrehozott sorok minimális százalékos körét.

    Példa:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int* (alapértelmezett: 25) 

    Mag számát határozza meg, hogy a algoritmus kezdeti helyi keresés pontok száma. Egyes esetekben az attól függően, hogy az adatokat, felépítésének mag számának növelése növeli szám (vagy minőség) nagyobb keresési terület lassabb lekérdezés útján címen keresztül az eredmények. A num_seeds érvet, csökkenő azt alatti 5 elérése elhanyagolható teljesítménybeli fejlesztések és 50 feletti növekvő ritkán készítése további minták csökkenő a találatok mindkét irányba.

    Példa:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < dupla < 1*+ (alapértelmezett: 0,5)

    Néhány általános (nagy követés) és a megfelelő (sok megosztott értékek) közötti egyensúly szabályozható ad. Növekvő size_weight általában csökkenti a minták számát, és minden minta általában nagyobb százalékos terjed ki. A további megosztott értékek és a kisebb százalékos követés szűkebb mintázatok size_weight általában csökkenő hoz létre. Az a motorháztető alatt hiperbolikusz képlete a súlyozott mértani középértékét a normalizált általános pontszám és a tájékoztató pontszámhoz size_weight és 1-size_weight, mint a súlyok között. 

    Példa:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*oszlopnév*

    Úgy véli, a bemeneti aszerint, hogy a megadott tömeg minden egyes sorához (alapértelmezés szerint minden egyes sorára van az egy vastagságának "1"), figyelembe kell figyelembe mintavételnél vagy bucketing és az adatok összesítése, amely már minden sorába beágyazott súly oszlop közös használatát.

    Példa:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>Kosár felmérése

     T | evaluate basket()

Kosár különálló attribútumok (méret) gyakori mintázatokat megkeresi az adatokat, és gyakori mintázatokat a gyakoriság küszöbértékét átadott az eredeti lekérdezést ad vissza. Kosár gyakori mintázatokat keresheti meg az adatokat a garantált, de nem biztos, hogy polinomiális futási idejű. Futási idejű a lekérdezés lineáris a sorok számát, de bizonyos esetekben lehet exponenciális (méret) oszlopok száma. Az eredetileg fejlesztett kosár analysis adatok adatbányászati Apriori algoritmus kosár alapul. 

**Adja eredményül.**

Több, mint az események megadott tört (alapértelmezett 0,05) szereplő összes mintázatok.

**Az argumentumok (az összes nem kötelező)**


* `threshold=`*0.015 < dupla < 1* (alapértelmezett: 0,05) 

    A sorok gyakori tekinteni minimális arányát akkor állítja be (mintázatok kisebb beállítása nem lesznek visszaadva).

    Példa:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*oszlopnév*

    Úgy véli, a bemeneti aszerint, hogy a megadott tömeg minden egyes sorához (alapértelmezés szerint minden egyes sorára van az egy vastagságának "1"), figyelembe kell figyelembe mintavételnél vagy bucketing és az adatok összesítése, amely már minden sorába beágyazott súly oszlop közös használatát.

    Példa: T |} basket("weight_column=sample_Count") felmérése


* `max_dims=`*1 < int* (alapértelmezett: 5)

    Használati kosár, csökkentheti a lekérdezés futtatókörnyezet alapértelmezés szerint korlátozott állítja be a predikátumban méretek maximális számát.


* `output=minimize` | `all` 

    Az eredmények formátum. A darab és a készültségi mindig megjelennek az eredmények.

 * `minimize`-oszlopok csak kiszűri "*" a találatok között.
 * `all`-a bemeneti az összes oszlop kimeneti.




#### <a name="evaluate-diffpatterns"></a>diffpatterns felmérése

     requests | evaluate diffpatterns("split=success")

Diffpatterns hasonlítja össze az azonos struktúra két adathalmaz és mintázatok különálló attribútumok (méret), a két adathalmaz közötti különbségek kétszélű értékét talál. Diffpatterns segíti a hibák (például úgy hibák az egy adott időkereten összehasonlítása hibák) elemzése kifejlesztett, de esetleg megtalálhatja a azonos struktúra bármely két adathalmaz közötti különbségek. 

**Szintaxis**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Adja eredményül.**

Diffpatterns (általában kicsi) lásson, a mintázatok, rögzítheti az adatokat a két különböző részeire (azaz mintát rögzítése az első adathalmaz sora nagy része és a sorok második megadása alacsony százalékos). Minden minta az eredményeket egy-egy sor jelöli.

Az első négy oszlop, hogy a darab és a sorok Kijelentkezés az eredeti lekérdezést, amely szerint a minta minden egyes készletben rögzítésének százalékát, az ötödik oszlop (abszolút százalékos eltérés pontban) a különbség a két adatcsoport között. Az eredeti lekérdezésből a többi oszlop és értékük, az oszlop vagy egy adott érték vagy * változóértékkel azaz. 

Figyelje meg, hogy a forgalmat ne legyenek különböző: Előfordulhat, hogy átfedik egymást, és általában nem foglalkozik az eredeti sorok. Bizonyos sorokat nem tartozhatnak bármely minta területen.

**Tippek**

* Hol használja, és a project a bemeneti függőleges vonás csökkentheti az adatokat, hogy éppen mi, amely érdekli.

* Amikor talál egy érdekes sor, érdemes lehet az adott értékeit szeretné szűrni a where hozzáadásával lehatolhatnak tovább.

**Argumentumok**

* `split=`*oszlopnév* (kötelező)

    Az oszlop pontosan két érték kell rendelkeznie. Ha szükséges, például oszlop létrehozása:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*karakterlánc*

    Ez a algoritmus csak meg a minták, amelynek van-e magasabb százalékértéket a cél adatkészlet, a cél kell lennie az az oszlop felosztása a két érték közül.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < dupla < 1* (alapértelmezett: 0,05) 

    Beállítja a minimális mintát (arány) különbség a két adatcsoport között.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Az eredmények formátum. A darab és a készültségi mindig megjelennek az eredmények. 

 * `minimize`-oszlopok csak kiszűri "*" a találatok között
 * `all`-a bemeneti az összes oszlop kimeneti

* `weight_column=`*oszlopnév*

    Úgy véli, a bemeneti aszerint, hogy a megadott tömeg minden egyes sorához (alapértelmezés szerint minden egyes sorára telepítette a egy vastagságának "1"). A közös súly oszlop használata fiók mintavételnél vagy bucketing és az adatok összesítése, amely már minden sorába beágyazott figyelembe.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>extractcolumns felmérése

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Egyszerű többhasábos, amelyek alapján típusukra dinamikusan kibontott ki (félig) strukturált oszlopoknak táblázat kiegészítése Extractcolumns szolgál. Jelenleg csak, json oszlopok mindkét dinamikus támogat és jsons szerializálása karakterlánc.


* `max_columns=`*int* (alapértelmezés: 10) 

    Dinamikus hozzáadott új oszlopok száma és nagyon nagy (om, pedig szó összes json-rekordot a distinct billentyűk számát) lehet úgy kell korlátozni, akkor azt. Az új oszlopoknak vannak rendezve, csökkenő sorrendben, a gyakoriság alapján, és felfelé max_columns bekerülnek a táblázatot.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*dupla* (alapértelmezett: 10.0) 

    Úgy is az új oszlopok figyelmen kívül hagyása az oszlopot, amelynek a gyakoriság argumentum kisebb, mint min_percent korlátozhatja.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*logikai* (alapértelmezett: igaz) 

    IGAZ, ha az összetett oszlopának a nevét hozzáadódik előtaggal a kibontandó oszlopokat nevét.


* `prefix_delimiter=`*karakterlánc* (alapértelmezett: "_") 

    Ha add_prefix = true a paraméter határoz meg a nevét az új oszlopok összefűzése használt elválasztó.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*logikai* (alapértelmezett: hamis) 

    IGAZ, ha az eredeti (json) oszlopokat az eredménytábla kell tartani.


* `output=query | table` 

    Az eredmények formátum. 

 * `table`-A kimeneti a táblázatból, a megadott beviteli oszlopok, valamint az új oszlopot, amely a bemeneti oszlopok voltak kiolvasott mínusz beérkező.
 * `query`-A kimeneti olyan karakterlánc, amely a lekérdezés a eredményét táblázatként tenné. 




### <a name="extend-operator"></a>Kijelölés méretének növelése a operátor

     T | extend duration = stopTime - startTime

Egy vagy több számított oszlop hozzáfűzése egy táblához. 


**Szintaxis**

    T | extend ColumnName = Expression [, ...]

**Argumentumok**

* *T:* A beviteli tábla.
* *ColumnName:* A hozzáadandó oszlopok kiválasztását neve. [Nevek](#names) tartalmazhat betűket,- és nagybetűk numerikus vagy "_" karaktereket. Használat `['...']` vagy `["..."]` idézőjelet kulcsszavak vagy más karakteres neveket.
* *Kifejezés:* A számítás a már meglévő oszlopok fölé.

**Adja eredményül.**

A beviteli tábla a megadott további oszlopok másolata.

**Tippek**

* Használat [`project`](#project-operator) helyett, ha is húzhatja, illetve nevezze át az egyes oszlopok.
* Nem használja az `extend` egyszerűen megszerezni a hosszú kifejezésekben használható rövidebb nevet. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    A táblázat a belső oszlopok indexelt; az új nevet egy újabb oszlopot, amely nem indexelt határozza meg, hogy a lekérdezés valószínűleg a lassabb.

**Példa**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>Csatlakozás operátor

    Table1 | join (Table2) on CommonColumn

Két tábla sorainak egyesíti által meghatározott oszlop egyező értékeket.


**Szintaxis**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumentumok**

* *Tábla1* – a "bal oldalt" az illesztés.
* *Tábla2* - "jobb oldalán lévő" az illesztés. Ez lehet egy beágyazott lekérdezést-kifejezés, amely a táblázat exportálja.
* *CommonColumn* - ugyanazt a nevet a két táblát tartalmazó oszlop.
* *Jellegű* – Itt adhatja meg, hogyan a két tábla sorai, amelyet össze kell vetni.

**Adja eredményül.**

A táblázat:

* Egy oszlop, minden oszlopának a két tábla, beleértve a megfelelő billentyűk. Az oszlopok jobb szélétől automatikusan átnevezésével neve ütközés esetén.
* Egy sor minden egyezés a beviteli tábla között. Egyezés kijelölve az egyik tábla minden azonos értéket tartalmazó sorok a `on` a másik tábla egy-egy sor a mezőket. 

* `Kind`Nincs megadva

    Csak egy sor bal szélén a minden érték megegyezik az `on` billentyűt. A kimenet minden egyező jobbról sort tartalmazó a sor egy sort tartalmaz.

* `Kind=inner`
 
     Van egy sor megfelelő sorok minden lehetséges kombinációját kimenetét a balra és jobbra.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     A belső egyezéseket kívül van minden sora a bal oldali (és/vagy a jobbra), a sor még akkor is, ha egyetlen egyező van. Ebben az esetben a nem egyezőket kereső kimeneti cellákban NULL értékek.

* `kind=leftanti`

     A bal oldalról nincs találat, jobbról a rekordokat adja vissza. Az eredményül kapott táblázat csak a bal oldalán lévő oszlopot tartalmaz. 
 
Ha több mezőkre vonatkozó ugyanazokkal az értékekkel, sorok szerezze be az összes kombinációi.

**Tippek**

A legjobb teljesítmény elérése érdekében:

* Használat `where` és `project` előtt csökkentheti a számokat, a sorok és oszlopok a bemeneti táblázatokban a `join`. 
* Ha egy tábla mindig kisebb, mint a többi, használnak az illesztés (védőeszközön) bal oldalán.
* Az illesztés egyezés az oszlopok azonos nevű kell rendelkeznie. A projekt operátorral táblára az oszlop átnevezése szükség esetén.

**Példa**

Tevékenységek első bővített az egyes bejegyzések be van jelölve a kezdő és a tevékenység befejező naplót. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>korlát operátor

     T | limit 5

A függvény eredménye a megadott számú sorok felfelé a beviteli tábla. Nincs semmilyen garanciát mely rekordot ad vissza. (Rekordok, használja a [`top`](#top-operator).)

**Alias**`take`

**Szintaxis**

    T | limit NumberOfRows


**Tippek**

`Take`egy egyszerű és hatékony módon egy mintája szerepel az eredmények megtekintéséhez a közös munka során interaktív van. Ne feledje, hogy azt nem garantálja kapcsol adott sorokat, vagy meghatározott sorrendben követik egymást létrehozásához.

Van egy implicit vonatkozó az ügyfél, a visszaadott sorok számát, akkor is, ha Ön nem használja az `take`. Emelje fel ezt a korlátot, használja a `notruncation` ügyfél lehetőséget.



### <a name="mvexpand-operator"></a>mvexpand operátor

    T | mvexpand listColumn 

Egy lista cellából dinamikus beírt (JSON), hogy egyes külön sorokban kibontása A többi cellájára egy kibontott sor vannak létrejön egy újabb példánya. 

(Lásd még: [`summarize makelist`](#summarize-operator) ellentétes funkciót végzi, amely.)

**Példa**

Tegyük fel, a beviteli tábla:

|A:INT|B:String|D:Dynamic|
|---|---|---|
|1|"hello"|{"kulcs": "érték"}|
|2|"nemzetközi"|[0,1, "k", "v"]|

    mvexpand D

Eredmény a következő:

|A:INT|B:String|D:Dynamic|
|---|---|---|
|1|"hello"|{"kulcs": "érték"}|
|2|"nemzetközi"|0|
|2|"nemzetközi"|1|
|2|"nemzetközi"|"k"|
|2|"nemzetközi"|"v"|


**Szintaxis**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumentumok**

* *ColumnName:* Több sor az eredményben, oszlopban az elnevezett tömb van kibontva. 
* *ArrayExpression:* Egy tömb szorzására kifejezés. Ezen az űrlapon használja, ha egy új oszlopot szúr be, és megőrződik a meglévőt.
* *Neve:* Az új oszlop neve.
* *Typename:* Egy bizonyos típusú kibontott kifejezés árnyékot
* *RowLimit:* A létrehozott minden egyes eredeti sorában sorok maximális száma. Az alapértelmezett érték 128.

**Adja eredményül.**

Több sor bármelyik tömbben vagy a tömb kifejezésben a névvel ellátott oszlopban lévő értékek minden egyes.

A kibontott oszlop mindig rendelkezik dinamikus típusát. Például: használja a cast `todatetime()` vagy `toint()` Ha kiszámítania vagy összesített értékeket szeretne.

A tulajdonság-között kiterjesztések két módok használhatók:

* `bagexpansion=bag`: Az egy-bejegyzés tulajdonságcsomaghoz kibontva jelennek meg tulajdonságcsomaghoz. Ez az alapértelmezett bővítéséhez.
* `bagexpansion=array`: Tulajdonságcsomaghoz kibontva jelennek meg a két elemhez `[` *kulcs*`,`*érték* `]` struktúrákat, a tömb kulcsok és értékek egységes hozzáférést (, valamint, például fut a distinct count összesítési tulajdonság neve fölé). 

**Példák**


    exceptions | take 1 
  	| mvexpand details[0]

A Részletek mezőben az egyes tételeihez sorok egy kivétel rekord osztja.



### <a name="parse-operator"></a>elemzési operátor

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Értékek olvas karakterlánc. Egyszerű vagy normál kifejezés egyező használata.

**Szintaxis**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumentumok**

* `T`: A beviteli tábla.
* `kind`: 
 * `simple`(alapértelmezett): a `Match` karakterláncokat egyszerű karakterláncot.
 * `relaxed`: Ha a szöveg típusú oszlop nem elemezni, az oszlop értéke null és a elemzési továbbra is 
 * `regex`: a `Match` karakterláncokat reguláris kifejezéseket.
* `Text`: Egy oszlop vagy egyéb-kifejezés, amely az eredmény vagy átalakítható karakterlánc.
* *Hol.van:* A következő részét a megfelelő, és törli azt.
* *Oszlop:* Rendeljen a következő rész a karakterlánc ebben az oszlopban. Az oszlop jön létre, ha még nem létezik.
* *Típusa:* A következő részét a elemezni, a megadott típusú, például az int, dupla dátum. 


**Adja eredményül.**

A beviteli tábla, bővített szerint oszlopok listája.

Az elemek a `with` záradék egyező szemben a forrás szöveg viszont. Minden elem chews a forrás szöveg adattömb kikapcsolása: 

* Egy szövegkonstans vagy reguláris kifejezések a kurzor egyező az egyező hossza.
* A reguláris kifejezések egy regex elemzési használhatók az minimalizálásával operátor "?" Minél korábban Ugrás a következő egyezés.
* Az oszlop nevét, amelynek típusú, a megadott típusú elemzi a szöveget. Ha ilyen = mérsékelten, egy sikertelen elemzési érvényteleníti a teljes mintának megfelelő.
* A típus nélkül, vagy a típus "karakterlánc", az oszlop nevét az alábbi egyező eléréséhez a minimális karakterszámot másolja.
* " *" Kihagyja az alábbi egyező eléréséhez a minimális karakterszámot. Használható "*" elején és végén a minta vagy azt követően a típusát eltérő karakterlánc vagy karakterlánc találatok között.

Az összes elemzési mintát elemeinek meg kell egyeznie megfelelően; Ellenkező esetben nem tartalmaz fog mutatni. A kivétel ez alól a szabály az, hogy ha jellegű = mérsékelten, ha nem sikerül egy beírt változó elemzése, a többi az elemzési továbbra is.

**Példák**

*Egyszerű:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | a számláló | bemutató | Év
---|---|---|---
1 | 2 | Socks | 63

*Enyhíteni:*

A bemeneti minden beírt oszlop megfelelő egyezés tartalmaz, amikor egy mérsékelten elemzési eredménye ugyanaz, mint egy egyszerű elemzési. De egyik beírt oszlop nem elemezhető helyesen, ha egy mérsékelten elemzési továbbra is a minta a többi feldolgozása, mivel az egy egyszerű elemzési leáll, ezért nem minden eredményt készítése.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | bemutató | Év
---|---|---
1 |  Socks | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

erőforrás | szeletek | zárolása | Megjelenés | előző
---|---|---|---|---
A Feladatütemező | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016 – 02-17T08:40:00Z

### <a name="project-operator"></a>Projekt operátor

    T | project cost=price*quantity, price

Jelölje ki a oszlopokat tartalmazza, átnevezésére, vagy húzza, és új számított oszlopok beszúrása. Az eredményben az oszlopok sorrendjének az argumentumok sorrendje határozza meg. Csak azokat az oszlopokat az argumentumként megadott szerepelnek az eredményben: megszakadnak a bemeneti bármely más.  (Lásd még: `extend`.)


**Szintaxis**

    T | project ColumnName [= Expression] [, ...]

**Argumentumok**

* *T:* A beviteli tábla.
* *ColumnName:* Az eredményben megjelenő oszlop neve. Ha a *kifejezés*nélkül, a név oszlop a bemeneti szerepelnie kell. [Nevek](#names) tartalmazhat betűket,- és nagybetűk numerikus vagy "_" karaktereket. Használat `['...']` vagy `["..."]` idézőjelet kulcsszavak vagy más karakteres neveket.
* *Kifejezés:* Hivatkozás a bemeneti oszlopokra skaláris kifejezés nem kötelező. 

    Egy olyan létező oszlopra azonos nevű új számított oszlopot a bemeneti szeretné jogi.

**Adja eredményül.**

Az oszlopok argumentumként, és a lehető legtöbb nevű táblázat sort tartalmaz, mint a beviteli tábla.

**Példa**

A következő példa bemutatja a különböző típusú elvégezhető műveletek használata a `project` operátor. A beviteli tábla `T` típusú három oszlopot tartalmaz `int`: `A`, `B`, és `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>Project – nem vagyok a gépnél operátor

    T | project-away column1, column2, ...

Kizárja a megadott oszlopok. Az eredmény tetszés szerinti nevet kivételével az összes beviteli oszlopot tartalmaz.

### <a name="range-operator"></a>Tartományoperátor

    range LastWeek from ago(7d) to now() step 1d

Hozza létre az értékeket tartalmazó egyoszlopos táblázatot. Figyelje meg, hogy nincs-e a bemeneti folyamat. 

|LastWeek|
|---|
|a Skype 2015 – 12-05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|2015-12-12 09:10:04.627|



**Szintaxis**

    range ColumnName from Start to Stop step Step

**Argumentumok**

* *ColumnName:* Az egyetlen oszlop, a kimeneti tábla neve.
* *Indítása:* Az eredményben a legkisebb érték.
* *Leállítása:* A lehető legmagasabb értékre, a kimeneti (vagy a lehető legmagasabb értékre, ha ezt az értéket fölé lépések *lépés* egy határa) generálása.
* *Lépés:* Egymást követő két érték közötti különbség. 

Az argumentumok szám, dátum vagy időszak értékeket kell lennie. Ezek nem hivatkozhat olyan tábla oszlopainak. (Ha azt szeretné, a tartomány-beviteli tábla alapján számítja ki, használja a [tartomány *függvény*](#range), esetleg a [mvexpand operátor](#mvexpand-operator).) 

**Adja eredményül.**

A *ColumnName oszlopban*, amelynek értékei a következők *indítása*, *kezdje*nevű egyetlen oszlopot tartalmazó táblázat + *lépés*...-vel bezárólag *leállítása*.

**Példa**  

```AIQL
range Steps from 1 to 8 step 3
```

Egy egyetlen oszlopot tartalmazó táblázat neve `Steps` amelynek típusa `long` és értékekkel rendelkező `1`, `4`, és `7`.

**Példa**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Az utóbbi hét napban a éjfél táblázatát. A tároló (padló) függvény minden alkalommal, amikor csökkenti a kezdő napjának.

**Példa**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Látható hogyan a `range` operátor használható egy kis, alkalmi, majd használt bevezetésére nullákat hol forrásadatainak nem tartalmaz értéket dimenzió táblázat létrehozásához.

### <a name="reduce-operator"></a>operátor csökkentése

    exceptions | reduce by outerMessage

Próbálja meg egy hasonló rekordok csoportba. Az egyes csoportok, az operátor exportálja a `Pattern` , lapon, a legjobb ismerteti, hogy a csoport és a `Count` lévő rekordok.


![](./media/app-insights-analytics-reference/reduce.png)

**Szintaxis**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumentumok**

* *ColumnName:* Az oszlop szemügyre veszi. Karakterlánc típusú kell lennie.
* *Küszöb:* A {0.. 1 esik} tartományban található érték. Alapértelmezés szerint 0,001. Nagy bemeneti adatok alapján küszöb kis kell lennie. 

**Adja eredményül.**

Két oszlop, `Pattern` és `Count`. Sok esetben a minta az oszlop teljes érték lesz. Bizonyos esetekben akkor is azonosítsa a gyakori kifejezések és cserélje le változó részeket, ahol "*".

Eredménye például `reduce by city` tartalmazhatja: 

|Minta | Darabszám |
|---|---|
| San * | 5182 |
| Saint * | 2846 |
| Moszkvai | 3726 |
| \*-a-\* | 2730 |
| Párizs | 27163 |


### <a name="render-directive"></a>irányelv jeleníti meg

    T | render [ table | timechart  | barchart | piechart ]

Megjelenítés irányítja a bemutató réteg a tábla megjelenítése. Az utolsó eleme a függőleges vonás kell tenni. A kényelmes helyett a vezérlők megjelenítéséhez, amivel szeretné menteni a lekérdezés adott bemutató módszer.

### <a name="restrict-clause"></a>záradék korlátozása 

Itt adhatja meg az alábbi operátorok elérhető táblázatnevek csoportja. Példa:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>Rendezés operátor 

    T | sort by country asc, price desc

A beviteli tábla sorainak sorrendbe rendezése egy vagy több oszlop.

**Alias**`order`

**Szintaxis**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumentumok**

* *T:* A tábla rendezése a szövegbeviteli.
* *Oszlop:* A *T* szerint rendezni kívánt oszlop. Milyen típusú értékeket kell lennie, szám, dátum, idő vagy karakterláncot.
* `asc`Rendezés növekvő sorrendben; alacsonytól magas be. Az alapértelmezett érték `desc`, csökkenő magas, alacsony értékig.

**Példa**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Lekérdezés összes sorát, amelyek egy adott tábla halad `ActivityId`, azok időbélyeg szerint rendezett.

### <a name="summarize-operator"></a>Összegzés operátor

Egy olyan táblát, a tartalom a beviteli tábla összesítésének eredményez.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Egy táblázat, hogy a számot, átlag kérelem időtartam várost beállítása az egyes országok. Van egy sor az eredményben az egyes különböző országok. A kimeneti oszlopok megjelenítése a darabszám, átlag időtartam, városok és ország. A függvény figyelmen kívül hagyja a bemeneti oszlopot.


    T | summarize count() by price_range=bin(price, 10.0)

Egy táblázat, amely mutatja, hány van árak minden intervallum [0,10.0], [10.0,20.0], és így tovább. Ez a példa egy oszlop, a darabszám, és a ár tartomány rendelkezik. A függvény figyelmen kívül hagyja a bemeneti oszlopot.


**Szintaxis**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumentumok**

* *Oszlop:* Ha az eredmény oszlop nem kötelező neve. A kifejezés levezetni, az alapértelmezett nevet. [Nevek](#names) tartalmazhat betűket,- és nagybetűk numerikus vagy "_" karaktereket. Használat `['...']` vagy `["..."]` idézőjelet kulcsszavak vagy más karakteres neveket.
* *Összesítési:* Egy összesítő függvényt, például a hívásokat `count()` vagy `avg()`, az oszlopnevek argumentumként. [Összesítések](#aggregations)látható.
* *GroupExpression:* Egy kifejezést az oszlopra, amely biztosít egyedi értéket. Ez általában vagy az oszlop nevét, amely már tartalmaz korlátozott értékhalmaz, vagy `bin()` argumentum numerikus és az idő oszloppal. 

Ha megad egy numerikus és az idő kifejezést a használata nélkül `bin()`, Analytics automatikusan elhelyezi azt az időtartományt tartalmazó `1h` az esetekre, vagy `1.0` számokhoz.

Ha Ön nem adja meg a *GroupExpression,* a teljes táblázatot egy kimeneti sor összesített.



**Adja eredményül.**

A beviteli sor problémát az értékeket a csoportba vannak rendezve az `by` kifejezések. Kattintson a a megadott összesítő függvények számított minden csoport egy sor az egyes csoportok előállító fölé. Az eredmény tartalmazza a `by` oszlopok és is, ha legalább egy oszlop minden egyes számított összesítés. (Néhány összesítő függvények vissza több oszlopra.)

Az eredmény rendelkezik, ahány külön kombinációjának annyi sorok `by` értékeket. Ha szeretné összesíteni fölé numerikus értéket tartományát, `bin()` tartományok csökkentheti a diszkrét értékekre.

**Megjegyzés:**

Tetszőleges kifejezés biztosítanak az Összesítés és a csoportosítás kifejezések, bár is hatékonyabb egyszerű oszlopnevek, illetve `bin()` numerikus oszlopra.



### <a name="take-operator"></a>operátor készítése

Alias [határérték](#limit-operator)


### <a name="top-operator"></a>felső operátor

    T | top 5 by Name desc nulls first

Az első, a megadott oszlop szerint történő rendezésére *N* rekordot visszaad.


**Szintaxis**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumentumok**

* *NumberOfRows:* A *T* való visszatéréshez sorok számát.
* *Sort_expression:* Olyan kifejezés, amely a sorok rendezése. Érdemes általában csak az oszlop nevét. Megadhatja, hogy egynél több sort_expression.
* `asc`vagy `desc` (alapértelmezett) tűnhet, hogy vezérlő e érték a ténylegesen a "alsó" vagy "felső" a tartomány.
* `nulls first`vagy `nulls last` vezérlők hol jelennek meg a null értékeket. `First`az alapértelmezett `asc`, `last` alapértelmezett `desc`.


**Tippek**

`top 5 by name`egyenértékű `sort by name | take 5`. Fut gyorsabban és mindig vissza rendezve az eredményeket, mivel azonban `take` nem ilyen garantálja.

### <a name="top-nested-operator"></a>felső egymásba ágyazott operátor

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Eredményt hierarchikus, a korábbi szintű Lehatolási szintenként esetén. Hasznos lehet az, hogy a hang, például "Mik azok a leggyakoribb 5 kérelmek és az egyes őket, Mik azok a felső 3 teljesítmény időszakok, és az egyes őket, amely vannak a felső 3 országok kérelmek származnia?" kérdések megválaszolása

**Szintaxis**

   KÉTMINTÁS T |} felső egymásba ágyazott N az oszlopok szerinti ÖSSZESÍTÉS [, …]

**Argumentumok**

* N:INT – vissza és adják át a következő szintre sorok számát. Hol található az N a 5, a 3-as és a 3-as három szintű lekérdezés, a sorok száma lesz 45.
* OSZLOP - összesítésre csoportosítás oszlopot. 
* ÖSSZESÍTÉSI – a sorok egyes csoportjai alkalmazása egy [összesítő függvényt](#aggregations) . Az eredmények az alábbi összesítések határozza meg a felső csoportok megjeleníteni.


### <a name="union-operator"></a>Egyesítő operátor

     Table1 | union Table2, Table3

Két vagy több tábla tart, és az összeset sorát adja vissza. 

**Szintaxis**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumentumok**

* *Tábla1*, *tábla2* ...
 *  Az a tábla neve, például: `requests`, vagy egy [záradék engedélyezni](#let-clause); a definiált táblázatban vagy
 *  A lekérdezés kifejezés, például:`(requests | where success=="True")`
 *  A táblázatok helyettesítő meghatározott csoportja. Ha például `e*` az összes táblázatot az előző küldő záradékok, akiknek neve "e", a "kivétel" táblázat együtt kezdődött definiált Unió képezik.
* `kind`: 
 * `inner`-Az eredmény az oszlopokat, amelyeket az összes a beviteli tábla közös részhalmazát tartalmaz.
 * `outer`-Az eredmény bármely, a bemeneti adatok előforduló összes oszlopot tartalmaz. Nem egy beviteli sor által definiált cellák vannak beállítva, hogy `null`.
* `withsource=`*ColumnName:* Adja meg, ha a kimenet a *ColumnName oszlopban* , amelynek érték azt jelzi, hogy melyik forrást a tábla minden egyes sorára járult oszlopát fogja tartalmazni.

**Adja eredményül.**

Szerint tetszőleges számú sort tartalmazó táblázat a bemeneti táblázatokban, illetve annyi oszlopot, hogy egyedi oszlopnevei a bemeneti adatok alapján.

**Példa**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Az összes tábla, akiknek neve "tt" megkezdéséhez Unió.


**Példa**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
A különböző felhasználók előállított vagy száma egy `exceptions` esemény vagy egy `traces` eseményt a múltbeli nap során. Az eredményben a "SourceTable" oszlop "Lekérdezés" vagy "Parancs" jelzi.

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Hatékonyabb verziójában ugyanazt az eredményt adja. A táblázat minden egyes szűri az Unió létrehozása előtt.

### <a name="where-operator"></a>Ha operátor

     requests | where resultCode==200

Szűri a táblát, sorok, amelyek megfelelnek a predikátumok részét.

**Alias**`filter`

**Szintaxis**

    T | where Predicate

**Argumentumok**

* *T:* A táblázatos bemeneti, amelynek rekordjait szűrni vannak.
* *Predikátumalakzat:* A `boolean` [kifejezés](#boolean) *T*oszlopnyi fölé. *T*minden egyes sorához kiértékelt azt.

**Adja eredményül.**

Sorok *T* , amelynek *predikátumalakzat* nem `true`.

**Tippek**

Lépjen a leggyorsabb teljesítményt:

* **Egyszerű használat-összehasonlító táblázat** között az oszlopnevek és állandók. ("Folyamatos" azt jelenti, hogy állandó során a táblázatban – így `now()` és `ago()` az OK gombra, és így skaláris értéket rendeli használata egy [ `let` záradék](#let-clause).)

    Ha például inkább `where Timestamp >= ago(1d)` való `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest feltételek első**: Ha egyszerre több záradékot tartalmazó conjoined `and`, először helyezi el a záradékot, amely magában foglalja a csak egy oszlopot. Így `Timestamp > ago(1d) and OpId == EventId` célszerűbb, mint fordítva.


**Példa**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Azok a rekordok, amelyek nem régebbi 1 óra értéket, mint a forrásból, úgynevezett "Kuskus" és az azonos értékű két oszlop van. 

Figyelje meg, hogy azt helyezni az összehasonlítás két oszlop között utolsó, nem használja az index és a keresés kezd.


### <a name="where-in-operator"></a>Ha az operátor

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Szintaxis**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumentumok**

* `col`: A táblázat egy oszlop.
* `expr1`...: Skaláris kifejezés listáját.

Használati `in` használatos, ha meg szeretné jeleníteni csak azokat a sorokat, amelyben `col` a kifejezések egyenlő `expr1...`.

Használat `!in` Ha meg szeretné jeleníteni csak azokat a sorokat, amelyben `col` értéke nem egyezik meg valamelyik, az kifejezések `expr1...`.  


## <a name="aggregations"></a>Összesítések

Összesítések függvényeket a [művelet összegzés](#summarize-operator)létrehozott csoportok értékek egyesítéséhez. Ebben a lekérdezésben, például a dcount() is egy összesítő függvényt:

    requests | summarize dcount(name) by success

### <a name="any"></a>bármely 

    any(Expression)

Véletlenszerűen a csoport egy sor kijelölése, és a megadott kifejezés értékét adja eredményül.

Ez akkor hasznos, például amikor az egyes oszlop sok hasonló értékek (például "Hiba szöveg" oszlopban) és az oszlop minden egy egyedi érték, a csoport összetett kulcs egyszer mintát szeretne. 

**Példa**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Sor megtalálja a csoport, így a kis méretűre állítása és maximalizálja *ExprToMaximize* *ExprToReturn* értékét adja eredményül (vagy `*` való visszatéréshez a teljes sor).

**Tipp**: az átadott keresztül oszlopokat a program automatikusan átnevez. Győződjön meg arról, hogy a megfelelő neveket esetén, hogy vizsgálja meg az eredmények segítségével `take 5` előtt be egy másik operátor pipe meg az eredményeket.

**Példák**

Az egyes kérelem nevének megjelenítése a mikor történt, a leghosszabb kérelmet:

    requests | summarize argmax(duration, timestamp) by name

A leghosszabb kérelem, az időbélyegző nem csak minden részlet megjelenítése:

    requests | summarize argmax(duration, *) by name


Keresse meg a legkisebb érték, mindegyik mérőszám, az időbélyegző és más adatok együtt:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>AVG

    avg(Expression)

Kiszámítja a csoport keresztül *kifejezés* átlagát.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

A minimális sémának *DynamicExpression*az összes érték beengedi lekérdezése 

A paraméter oszloptípus kell `dynamic` – egy tömb vagy tulajdonság bag. 

**Példa**

    exceptions | summarize buildschema(details)

Eredmény:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Figyelje meg, hogy `indexer` szeretne megjelölni, hogy hol kell használni egy numerikus index használják. A séma néhány az elérési utak volna kell (feltételezve, hogy példa indexeknek tartomány):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Példa**

Tegyük fel, a bemeneti oszlop értéke három dinamikus értékeket:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Az eredményül kapott séma a következő lesz:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

A séma közli velünk, hogy:

* A legfelső objektum nevű négy tulajdonságok tároló x, y, z-t.
* A tulajdonság neve "x", amely a jövőben típusú "int" vagy "karakterlánc" típusú.
* A tulajdonság neve "i", "dupla" típus bármelyikének sikerült, vagy egy másik tároló egy tulajdonság neve "w", "karakterlánc" típusú.
* A ``indexer`` kulcsszó azt jelzi, hogy "z" és "t" tömbök.
* A "z" tömb minden elem vagy az int, vagy a karakterlánc.
* "t" karakterláncok tömbje.
* Minden tulajdonság implicit módon nem kötelező, és bármelyik a tömb üres is lehet.

##### <a name="schema-model"></a>Séma modell

A visszaadott séma szintaxisa a következő:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Azok a egyenértékű a dinamikus értékként kódolt géppel típus széljegyzetek egy részét. A példa séma géppel, a következő lesz:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>Darabszám

    count([ Predicate ])

A számát adja eredményül, amelynek *predikátumalakzat* az eredmény sorok `true`. Ha nincs *predikátumalakzat* megadva, akkor a rekordok száma a csoport adja eredményül. 

**Perf tipp**: használata `summarize count(filter)` helyett`where filter | summarize count()`

> [AZURE.NOTE] Kerülje count() kérelmeket, kivételek vagy más események történtek számának megállapítására. Amikor a művelet [mintavételnél](app-insights-sampling.md) , tartja meg az alkalmazás háttérismeretek az adatpontok száma kisebb, mint az eredeti események száma lesz. Ehelyett, `summarize sum(itemCount)...`. A elemek száma tulajdonság tükrözi, amely az egyes megőrzött adatpont jelző eredeti események száma.

### <a name="countif"></a>DARABTELI

    countif(Predicate)

A számát adja eredményül, amelynek *predikátumalakzat* az eredmény sorok `true`.

**Perf tipp**: használata `summarize countif(filter)` helyett`where filter | summarize count()`

> [AZURE.NOTE] Kerülje countif() kérelmeket, kivételek vagy más események történtek számának megállapítására. Amikor a művelet [mintavételnél](app-insights-sampling.md) , az adatpontok száma kisebb, mint a tényleges események száma lesz. Ehelyett, `summarize sum(itemCount)...`. A elemek száma tulajdonság tükrözi, amely az egyes megőrzött adatpont jelző eredeti események száma.

### <a name="dcount"></a>AB.darab

    dcount( Expression [ ,  Accuracy ])

A csoport becslése *kifejezés* a különböző értékek számát adja eredményül. (A különböző értékek listáját, használjon [`makeset`](#makeset).)

*Pontosabb*, adja meg, ha sebességétől és a pontosság közötti egyensúly szabályozza.

 * `0`a legkevésbé pontos és leggyorsabb számítás =.
 * `1`az alapértelmezett, amely pontosságát és a számítási idő; információ 0.8 %-os hiba.
 * `2`= leginkább pontos és legkisebb számítási; információ 0,4 %-os hiba.

**Példa**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

A *kifejezés* a sorok a különböző értékek száma becslése a csoportot, amelynek *predikátumalakzat* értéke igaz adja eredményül. (A különböző értékek listáját, használjon [`makeset`](#makeset).)

*Pontosabb*, adja meg, ha sebességétől és a pontosság közötti egyensúly szabályozza.

 * `0`a legkevésbé pontos és leggyorsabb számítás =.
 * `1`az alapértelmezett, amely pontosságát és a számítási idő; információ 0.8 %-os hiba.
 * `2`= leginkább pontos és legkisebb számítási; információ 0,4 %-os hiba.

**Példa**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Vissza a `dynamic` (JSON) tömbje *kifejezés* csoportjának az összes érték. 

* *MaxListSize* egy választható egész korlátot, a visszaadott elemek maximális száma (az alapértelmezett érték *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Vissza a `dynamic` (JSON) tömb az egyedi értéket, amely a csoport *kifejezés* közül. (Tipp: a különböző értékek csak megszámolásához használja [`dcount`](#dcount).)
  
*  *MaxSetSize* visszaadott elemek maximális száma egy választható egész vonatkozó (az alapértelmezett érték *128*).

**Példa**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Lásd még: a [ `mvexpand` operátor](#mvexpand-operator) a ellenkező függvény.


### <a name="max-min"></a>Max, min

    max(Expr)

A maximális számát a *kifejezés*kiszámítja.
    
    min(Expr)

Kiszámítja a *kifejezés*a minimális.

**Tipp**: Ez kínál fel a min és max saját – például a legnagyobb vagy legkisebb ár. De ha azt szeretné, hogy a többi oszlop a sor -, például a legalacsonyabb ár a szolgáltató neve - [argmin vagy argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>PERCENTILIS, percentilisek, percentilew, percentilesw

    percentile(Expression, Percentile)

A megadott PERCENTILIS csoportjának *kifejezés* becslést ad eredményül. A pontosság attól függ, hogy a PERCENTILIS a tartományban lévő sokasága, a sűrűségfüggvény eredménye.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Például `percentile()`, de PERCENTILIS értékek száma, (ami külön-külön kiszámítása az egyes PERCENTILIS gyorsabb) számítja ki.

    percentilew(Expression, WeightExpression, Percentile)

Súlyozott percentilise. Ezzel az előre összesített adatok.  `WeightExpression`az egész szám, amely azt jelzi, hogy hány eredeti sorokat jeleníti meg az egyes összesített sor.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Például `percentilew()`, de a PERCENTILIS értékek számát számítja ki.

**Példák**


Értékének `duration` , hogy ez a minta beállítása és a minta megadása, az egyes kérelem nevének számított 5 %-nál kisebb 95 %-nál nagyobb:

    request 
  	| summarize percentile(duration, 95)
      by name

Kihagyása "által..." szeretné kiszámítani a teljes táblázatot.

Több percentilisek különböző kérelem nevek egyidejű kiszámítása:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Az eredmény, amely a kérelem /Events/Index számára, kérések 5 %-át vannak megválaszolt, a kisebb, mint 2.44s, fele őket a 3.52s, és 5 %-os 6.85s lassabb.

Több statisztika számítása:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Súlyozott percentilisek

A súlyozott PERCENTILIS függvény használjon esetekben, ahol a korábban már összesített előzetesen. 

Tegyük fel például, hogy az alkalmazás sok ezer másodpercenként műveleteket hajt végre, és meg szeretné ismerni a késés. Az egyszerű megoldást jelenthet egy alkalmazás háttérismeretek kérelem vagy egyéni esemény az összes művelet létrehozása. Adaptív mintavételnél érvénybe csökkentheti, bár ez hozna létre, a forgalom sok. De úgy dönt, hogy egy még jobb megoldás végrehajtása: az alkalmazását az adatok összesítése alkalmazás mélyebb elküldése előtt fog ír bizonyos kódot. Az összesített összefoglalásra szabályos időközönként, talán csökkentése a adatsebesség percenkénti néhány pontok küld.

A kód ezredmásodperc késést mérések adatfolyam vesz igénybe. Példa:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Megszámolja, hogy az a méreteket, a következő található tartalmat:`{ 10, 20, 30, 40, 50, 100 }`

Rendszeres időközönként TrackEvent hívásokat, az egyes gyűjtő minden hívás egyéni mérések az egyik sorozata van: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

Analytics megjelenik egy ilyen csoport a teljes jelennek meg:

`opCount` | `latency`| jelentése
---|---|---
8 | 10 | = Ha a 10ms mappában lévő 8 műveletek
6 | 20 | = a 20 MS mappában lévő 6 műveletek
3 | 30 | = Ha a 30ms mappában lévő 3 műveletek
1 | 40 | = Ha a 40ms mappában lévő 1 műveletek

Esemény késések eredeti eloszlását pontos képet, akkor használja `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Az azonos eredményt adnak, mintha egy egyszerű használt `percentiles` mérések eredeti beállítása.

> [AZURE.NOTE] Súlyozott percentilisek nem vonatkoznak a [adatok mintát](app-insights-sampling.md), ahol a mintában szereplő sornál eredeti sorokat, hanem a elemeket véletlen minta jelöli. Az egyszerű percentilisét, azaz függvények alkalmasak a mintában szereplő adatokat.

#### <a name="estimation-error-in-percentiles"></a>A százalékos érték becslés hiba

Percentilisek összesítés tartalmaz egy közelítő értéke, [T – kivonat](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf)használatával. 

Néhány fontos tudnivalókra: 

* A korlátok a becslés hibától a kért PERCENTILIS értékkel változnak. A legjobb pontosságát [0..100] végén méretezéséhez percentilisek 0 és 100 a pontos minimális és maximális értékek a eloszlás. A pontosság fokozatosan csökkenti a skála közepe felé. A medián a legrosszabb és 1 %-os eléri. 
* Hiba korlátok rangját, nem pedig az érték a betartják. Tegyük fel, hogy a percentilisét, azaz (X 50) Xm értékét adja vissza. A becslés garantálja, hogy legalább 49 és legfeljebb 51 % az X értékeket kisebbek Xm. Nincs elméleti korlátozva Xm és az X tényleges középérték közötti különbség.

### <a name="stdev"></a>szórás

     stdev(Expr)

A csoport alatt a *kifejezés* szórását adja eredményül.

### <a name="variance"></a>eltérés

    variance(Expr)

A csoport alatt *kifejezés* varianciáját adja eredményül.

### <a name="sum"></a>összeg

    sum(Expr)

A csoport alatt *kifejezés* összegét adja eredményül.                      


## <a name="scalars"></a>Scalars

[kiterjesztések](#casts) | [összehasonlítása](#scalar-comparisons)
<br/>
[GETTYPE](#gettype) | [kivonat](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

Támogatott fájltípusok a következők:

| Típus      | További neve   | Egyenértékű .NET-típus |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Kiterjesztések

Egy típusból között átalakítva. Az általános a konvertálás van ilyesmire lehetőség, ha működik:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Ellenőrizze, hogy egy karakterlánc átalakítható-e a megadott típusú:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skaláris összehasonlítása

||
---|---
`<` |Kisebb
`<=`|Kisebb vagy egyenlő
`>` |Nagyobb
`>=`|Nagyobb vagy egyenlő
`<>`|Nem egyenlő
`!=`|Nem egyenlő 
`in`| Jobb oldali operandus egy (dinamikus) tömb pedig bal oldali operandus elemei közül.
`!in`| Jobb oldali operandus egy (dinamikus) tömb pedig bal oldali operandus nem egyenlő elemei közül.




### <a name="gettype"></a>GETTYPE

**Adja eredményül.**

Egyetlen argumentum a mögöttes tároló típusú képviselő karakterlánc. Ez akkor különösen hasznos, ha típusú értékeket `dynamic`: Ebben az esetben `gettype()` mutatni fogja azokat, hogyan érték van kódolva.

**Példák**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>kivonat

**Szintaxis**

    hash(source [, mod])

**Argumentumok**

* *forrás*: A forrás skaláris a kivonat számolja ki.
* *maradék*: A moduló érték, akkor alkalmazza a kivonat között.

**Adja eredményül.**

Az adott skaláris moduló az adott maradék érték (ha van megadva) xxhash (hosszú) értéke.

**Példák**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

A `iff()` függvény első argumentumában (predikátumok) ad, és attól függően, hogy a predikátumok van-e a második vagy a harmadik argumentum értékét adja eredményül `true` vagy `false`. A második és harmadik argumentum típusával egyező típusú kell lennie.

**Szintaxis**

    iff(predicate, ifTrue, ifFalse)


**Argumentumok**

* *predikátumok:* Egy kifejezés, amely az eredmény egy `boolean` értéket.
* *ifTrue:* Kap kiértékelt kifejezés és annak értékét adja vissza a függvény a Ha *predikátumok* ad `true`.
* *ifFalse:* Kap kiértékelt kifejezés és annak értékét adja vissza a függvény a Ha *predikátumok* ad `false`.

**Adja eredményül.**

Ez a funkció *ifTrue* értékét adja eredményül, ha az eredmény *predikátumok* `true`, a visszaadott érték, különben a program *ifFalse* .

**Példa**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, isnotnull, notnull

    isnull(parsejson("")) == true

Egyetlen argumentuma tart, és azt jelenti, hogy értéke null.

**Szintaxis**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Adja eredményül.**

IGAZ vagy hamis attól függően, hogy az értéke null vagy nem null értéket.


|x|IsNull(x)
|---|---
| "" | hamis
|"x" | hamis
|parsejson("")|Igaz
|parsejson("[]")|hamis
|parsejson("{}")|hamis

**Példa**

    T | where isnotnull(PossiblyNull) | count

Figyelje meg, hogy más módokon elérésének erről:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Lekérdezés vagy a kifejezés eredménye, és az eredményt adja, mint egy egyedi érték. Ez a funkció akkor hasznos, ha a szakaszos számítások; események teljes számának kiszámítása és a, majd az alapterv használ.

**Szintaxis**

    toscalar(query)
    toscalar(scalar)

**Adja eredményül.**

A kiértékelt argumentumban. Ha az argumentum egy táblázat, adja vissza az első sor első oszlopában. (Rendezheti, hogy a argumentum rendelkezik, csak egy üres sort és oszlopot a javasolt.)

**Példa**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Logikai literálok

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Logikai operátorok

    and 
    or 

    

## <a name="numbers"></a>Számok

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Numerikus literálok

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Számtani operátorok

|| |
|---|-------------|
| + | Hozzáadása         |
| - | Kivonása    |
| * | Szorzás    |
| / | Osztása      |
| % | Moduló      |
||
|`<` |Kisebb
|`<=`|Kisebb vagy egyenlő
|`>` |Nagyobb
|`>=`|Nagyobb vagy egyenlő
|`<>`|Nem egyenlő
|`!=`|Nem egyenlő 


### <a name="abs"></a>ABS

**Szintaxis**

    abs(x)

**Argumentumok**

* x – egy egész szám, valós vagy időszak

**Adja eredményül.**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>mappa, a floor

Kerekítése lefelé egy egész értéket az egy adott bin méret több. Használt sokkal a [`summarize by`](#summarize-operator) lekérdezést. Ha szórt értékhalmaz, azok konkrét értékek kevesebb beállítási almappákba lesz csoportosítva.

Alias `floor`.

**Szintaxis**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumentumok**

* *érték:* Egy szám, dátum vagy időszak. 
* *roundTo:* A "mappa mérete". Egy számot, dátumot vagy időszak, hogy elosztja *értéket*. 

**Adja eredményül.**

A *roundTo* alatti *érték*legközelebb eső többszörösére.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Példák**

Kifejezés | Eredmény
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


A következő kifejezés kiszámítja az 1 másodperc gyűjtő méretű időtartamok, a hisztogram:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>Exp

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>padló

Alias [`bin()`](#bin).

### <a name="gamma"></a>gamma

A [gamma-függvény](https://en.wikipedia.org/wiki/Gamma_function)

**Szintaxis**

    gamma(x)

**Argumentumok**

* *x:* Valós szám

Pozitív egész szám, a `gamma(x) == (x-1)!` például `gamma(5) == 4 * 3 * 2 * 1`.

Lásd még: [loggamma](#loggamma).


### <a name="log"></a>napló

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`a valós szám > 0 kell lennie. Null ad vissza.

### <a name="loggamma"></a>loggamma


Az abszolút értékét a [gamma-függvény](#gamma)természetes logaritmusa.

**Szintaxis**

    loggamma(x)

**Argumentumok**

* *x:* Valós szám


### <a name="rand"></a>VÉL függvény

A véletlen szám nyilvántartás-készítő alkalmazás.

* `rand()`-0.0 és 1,0 közötti valós szám
* `rand(n)`-0 és n-1 közötti egész szám




### <a name="sqrt"></a>gyök

A négyzetgyökét függvényt.  

**Szintaxis**

    sqrt(x)

**Argumentumok**

* *x:* Valós szám > = 0.

**Adja eredményül.**

* Pozitív szám, hogy az`sqrt(x) * sqrt(x) == x`
* `null`Ha az argumentum értéke negatív, vagy nem konvertálható egy `real` értéket. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Dátum és idő


[korábbi](#ago) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [most](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Dátum és idő literálok

|||
---|---
**dátum és idő**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Időpontok állandóan UTC szerint megadva. Ma egyszerre kimarad a dátumot adja vissza.
`now()`|A pontos idő.
`now(`-*időszak*`)`|`now()-`*időszak*
`ago(`*időszak*`)`|`now()-`*időszak*
**időszak**|
`2d`|2 nap
`1.5h`|1,5 óra 
`30m`|30 percben
`10s`|10 másodperc
`0.1s`|a második 0.1-es
`100ms`| 100 ezredmásodperc
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| 2 nap
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Dátum és idő kifejezések

Kifejezés |Eredmény
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Kisebb
`<=`|Kisebb vagy egyenlő
`>` |Nagyobb
`>=`|Nagyobb vagy egyenlő
`<>`|Nem egyenlő
`!=`|Nem egyenlő 




### <a name="ago"></a>korábbi

A jelenlegi UTC óra időpontot a megadott időszak kivonja. Például `now()`, ez a funkció használható többször utasításokban és a hivatkozott UTC órája ugyanaz az összes megvalósításokkal lesz.

**Szintaxis**

    ago(a_timespan)

**Argumentumok**

* *a_timespan*: intervallum kivonni azokból az aktuális UTC Óra idő (`now()`).

**Adja eredményül.**

    now() - a_timespan

**Példa**

Az elmúlt órában időbélyeg az összes sort:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Egy egész szám, dátum adott részét kibontása.

**Szintaxis**

    datepart(part, datetime)

**Argumentumok**

* `part:String`-{"Year", "Hónap", "Nap", "Óra", "Perc", "Második", "Ezredmásodperc", "Mikroszekundum", "Nanoszekundumos"}
* `datetime`

**Adja eredményül.**

A hosszú, amely az adott részét.


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

A hónap napja sorszámmal.

**Szintaxis**

    dayofmonth(a_date)

**Argumentumok**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>DayOfWeek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Az egész szám, a megelőző óta eltelt napok száma a vasárnap, mint egy `timespan`.

**Szintaxis**

    dayofweek(a_date)

**Argumentumok**

* `a_date`: A `datetime`.

**Adja eredményül.**

A `timespan` óta az előző elején éjfél vasárnap lefelé kerekíti a napok egy egész szám.

**Példák**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DAYOFYEAR

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Az év napjának sorszámmal.

**Szintaxis**

    dayofyear(a_date)

**Argumentumok**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday, endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getmonth

Egy datetime megnyithatja a hónap száma (1 – 12).

**Példa**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Az év letölthető datetime.

**Példa**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>most

    now()
    now(-2d)

Az aktuális UTC Óra idő tetszés szerint eltolása egy adott időszak szerint. Ez a funkció is használható többször utasításokban és a hivatkozott órája összes előfordulását azonos lesz.

**Szintaxis**

    now([offset])

**Argumentumok**

* *eltolás:* A `timespan`, az aktuális UTC Óra idő hozzáadott. Alapértelmezett: 0.

**Adja eredményül.**

Az aktuális UTC óra egyidejűleg egy `datetime`.

    now() + offset

**Példa**

Az esemény, a kulcsszó jelölt óta határozza meg, hogy az intervallum:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday, startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Ellenőrizze, hogy egy karakterlánc egy érvényes dátumot:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Az egész eredmény szerint a szabványos ISO 8601 a hét számát jelöli. A hét első napja vasárnap, az év első hetének pedig a hét, amely tartalmazza az adott év első csütörtök. (Év utolsó napja ezért tartalmaz néhány a következő év 1 hét azon napjait vagy az első nap néhány hét 52 vagy az előző év 53 is tartalmazhat.)


## <a name="string"></a>Karakterlánc

[countof](#countof) | [kibontásához](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [cseréje](#replace) | [felosztása](#split) | [strcat](#strcat) | [strlen](#strlen) | [karakterlánc](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Szövegkonstanssal

A szabályok mindig ugyanazok a JavaScript.

Karakterláncok lehet idézőjelek közé, szimpla vagy dupla idézőjel-karakterek. 

Fordított perjel (`\`)-alapú például escape-karakterek `\t` (lap) `\n` (újsor) és idézőjel mellékelve példányát.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Kódolt szövegkonstanssal

Kódolt szövegkonstanssal olyan karakterláncok, amelyek a Analytics fog takarják a dátumok a karakterlánc (például követésekor). A címmódosítás folyamat felülírja az összes kódolt karakterek által a kezdő (`*`) karaktert.

Az űrlap-kódolt karakterláncot, hogy prepend `h` vagy "H". Példa:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Karakterláncok összehasonlításakor

Műveleti jel|Leírás|Kis-és nagybetűk|Igaz példa
---|---|---|---
`==`|Egyenlő |igen| `"aBc" == "aBc"`
`<>` `!=`|Nem egyenlő|igen| `"abc" <> "ABC"`
`=~`|Egyenlő |nem| `"abc" =~ "ABC"`
`!~`|Nem egyenlő |nem| `"aBc" !~ "xyz"`
`has`|Jobb-oldali melletti (jobb oldal), a bal oldalon-oldali melletti (LHS) teljes kifejezés|nem| `"North America" has "america"`
`!has`|Jobb oldal nem egy teljes LHS kifejezéshez|nem|`"North America" !has "amer"` 
`hasprefix`|Jobb oldal a kifejezés LHS előtaggal|nem|`"North America" hasprefix "ame"`
`!hasprefix`|Jobb oldal nem minden szerződési LHS előtaggal|nem|`"North America" !hasprefix "mer"`
`hassuffix`|Jobb oldal LHS kifejezés toldalékot|nem|`"North America" hassuffix "rth"`
`!hassuffix`|Jobb oldal nem minden szerződési LHS utótag|nem|`"North America" !hassuffix "mer"`
`contains` | Jobb oldal akkor fordul elő, mint egy részét LHS|nem| `"FabriKam" contains "BRik"`
`!contains`| Jobb oldal nem jelentkezik lhs|nem| `"Fabrikam" !contains "xyz"`
`containscs` | Jobb oldal akkor fordul elő, mint egy részét LHS|igen| `"FabriKam" contains "Kam"`
`!containscs`| Jobb oldal nem jelentkezik lhs|igen| `"Fabrikam" !contains "Kam"`
`startswith`|Jobb oldal, egy kezdeti LHS részét.|nem|`"Fabrikam" startswith "fab"`
`!startswith`|Jobb oldal nem egy LHS első részét.|nem|`"Fabrikam" !startswith "abr"`
`endswith`|Jobb oldal egy LHS Terminálszolgáltatások részét.|nem|`"Fabrikam" endswith "kam"`
`!endswith`|Jobb oldal nem egy LHS Terminálszolgáltatások részét.|nem|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS jobb oldal egyezés tartalmaz|igen| `"Fabrikam" matches regex "b.*k"`
`in`|Az elemek egyenlő|igen|`"abc" in ("123", "345", "abc")`
`!in`|Az elemek közül nem egyenlő|igen|`"bc" !in ("123", "345", "abc")`

Használat `has` vagy `in` Ha, egész lexikai kifejezés - jelenlétét vizsgálata esetén ez azt jelenti, hogy szimbólum vagy egy alfanumerikus word határolt nem alfanumerikus karaktereket vagy indítása vagy a mező végére. `has`hajt végre gyorsabb `contains`, `startswith` vagy `endswith`. Ezek a lekérdezések első gyorsabban futtatható:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Egy karakterlánc, egy karakterlánc előfordulását megszámolja. Egyszerű karakterlánc egyezéseket előfordulhat, hogy átfedik egymást; Regex egyezéseket nem.

**Szintaxis**

    countof(text, search [, kind])

**Argumentumok**

* *szöveg:* Egy karakterlánc.
* *keresési:* Az egyszerű karaktersorozat vagy kifejezéssel belüli *szöveg*megfelelően.
* *milyen:* `"normal"|"regex"` Alapértelmezett `normal`. 

**Adja eredményül.**

Az a keresőkifejezést össze kell vetni a tároló száma. Egyszerű karakterlánc egyezéseket előfordulhat, hogy átfedik egymást; Regex egyezéseket nem.

**Példák**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (nem 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>kibontása

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

A [reguláris kifejezésekkel](#regular-expressions) egyezés kinyerése egy szöveges karakterlánc. Tetszés szerint, majd alakítja át a kibontott karakterlánc jelzett típusát.

**Szintaxis**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumentumok**

* *regex:* [Reguláris kifejezések](#regular-expressions).
* *captureGroup:* Egy pozitív `int` állandó jelző a rögzítési csoport kibontásához. az egész egyező értéket a megfelel-e a kifejezéssel, 2 vagy több egymást követő zárójelek a az első '("zárójel")' 1 0 jelöli.
* *szöveg:* A `string` kereséséhez.
* *typeLiteral:* Egy választható típusú literál (például `typeof(long)`). Ha megadták, a kibontott karakterlánc típus alakul. 

**Adja eredményül.**

Ha *regex* egyezést *szövegben*: azon részét, szemben a megjelölt rögzítési csoport *captureGroup*egyező *typeLiteral*tetszés szerint alakulnak.

Ha nem szerepel, vagy a típuskonverziós hiba jelenik meg: `null`. 

**Példák**

A példa karakterlánc `Trace` kell keresni definícióját `Duration`. Az egyező alakul `real`, majd, szorozva az idő állandó (`1s`), hogy `Duration` típusa `timespan`. Ebben a példában az egyenlő 123.45 másodperc:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

Ez a példa függvény egyenértékű a `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty, isnotempty, notempty

    isempty("") == true

IGAZ, ha az argumentum egy üres karakterlánc, vagy értéke null.
Lásd még: [isnull](#isnull).


**Szintaxis**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Adja eredményül.**

Azt jelzi, hogy a argumentum egy üres karakterláncot vagy isnull.

|x|IsEmpty(x)
|---|---
| "" | Igaz
|"x" | hamis
|parsejson("")|Igaz
|parsejson("[]")|hamis
|parsejson("{}")|hamis


**Példa**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

Részekre URL-címet a.

**Szintaxis**

    parseurl(urlstring)

**Argumentumok**

* *urlstring:* EGY URL-CÍMÉT.

**Adja eredményül.**

A kijelzők karakterláncként tartalmazó objektum.

**Példa**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>Csere

Összes regex találat cseréje másik karakterlánc.

**Szintaxis**

    replace(regex, rewrite, text)

**Argumentumok**

* *regex:* A [reguláris kifejezésekkel](https://github.com/google/re2/wiki/Syntax) *szöveg*kereséséhez. Rögzítés csoportok zárójelben"(" ")" tartalmazhat. 
* *átírásának:* A csere regex az *matchingRegex*bármely egyezés. Használati `\0` hivatkozni kell a teljes egyezés `\1` rögzítése az első csoport `\2` és így tovább későbbi rögzítési csoportok számára.
* *szöveg:* Egy karakterlánc.

**Adja eredményül.**

*szöveg* *regex* az összes találat helyettesítve *átírásának*értékelése után. Találatok nem áll átfedésben.

**Példa**

Ez az utasítás:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

A következő eredményeket foglalja magában:

| x    | Str | helyett|
|---|---|---|
| 1    | Értéke 1.000000  | Szám volt: 1.000000|
| 2    | Értéke 2.000000  | Szám volt: 2.000000|
| 3    | Értéke 3.000000  | Szám volt: 3.000000|
| 4    | Értéke 4.000000  | Szám volt: 4.000000|
| 5    | Értéke 5.000000  | Szám volt: 5.000000|
 



### <a name="split"></a>felosztása

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Kettéválaszthatja-e egy adott karakterlánc adott elválasztó szerint, és a conatined összefűzendő karakterláncokat egy karakterlánc tömböt adja eredményül. Tetszés szerint egy adott karakterlánc is lehet visszaadott Ha létezik.

**Szintaxis**

    split(source, delimiter [, requestedIndex])

**Argumentumok**

* *forrás*: A forrás-karakterlánc, amely szerint az adott határoló a szétválasztott lesz.
* *elválasztó*: A annak érdekében, hogy a forrás-karakterlánc felosztása használandó elválasztó.
* *requestedIndex*: nem kötelező, nulla alapú index `int`. Ha megadták, a visszaadott karakterlánc tömb a kért karakterlánc fog tartalmazni, ha létezik. 

**Adja eredményül.**

Az összefűzendő karakterláncokat az adott forrás karakterlánc, amely az adott elválasztó karakter vannak tagolva tartalmazó karakterlánc tömböt.

**Példák**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Összefűzi a közötti 1 és 16 argumentumok karakterláncok kell lennie.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Karakterlánc hosszának.

### <a name="substring"></a>karakterlánc

    substring("abcdefg", 1, 2) == "bc"

Egy karakterlánc kinyerése egy adott indexben kezdve adott forrás karakterlánc. Másik lehetőségként a kért karakterlánc hossza adható meg.

**Szintaxis**

    substring(source, startingIndex [, length])

**Argumentumok**

* *forrás:* A forrás-karakterlánc, amely a karakterlánc veszi át.
* *startingIndex:* A nulla alapú kezdő karakter pozícióját a kért karakterlánc.
* *hossza:* Nem kötelező paraméter adhat meg a kért számú karakter a karakterlánc használható. 

**Adja eredményül.**

Egy karakterlánc, az adott pozíciójától. A karakterlánc Karakterpozíció startingIndex (nulla alapú) kezdődik, és a karakterlánc hossza karakterből végére továbbra is fennáll, ha a megadott.

**Példák**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>ToLower

    tolower("HELLO") == "hello"

Kisbetűsre alakítja.

### <a name="toupper"></a>ToUpper

    toupper("hello") == "HELLO"

A nagybetűs alakítja.



### <a name="guids"></a>GUID

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Tömbök, objektumok és dinamikus

[szövegkonstansok](#dynamic-literals) | [döntő](#casting-dynamic-objects) | [operátorok](#operators) | [záradékok engedélyezése](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [tartomány](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Az alábbiakban a kivételt alkalmazás az összefüggéseket a lekérdezés eredményét. Az érték `details` tömböt.

![](./media/app-insights-analytics-reference/310.png)

**Indexelés:** Index tömbök és objektumok JavaScript hasonlóan:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* De `arraylength` és más Analytics függvények (nem ".length"!)

**Leadó** Bizonyos esetekben leadott egy objektumból kinyerte elemet, mert típusának megfelelő eltérőek lehetnek szükség. Ha például `summarize...to` van szüksége a megadott típusú:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Szövegkonstansok** Közvetlen tömb vagy tulajdonság-között objektum létrehozásához írja olyan JSON karakterlánc és cast:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Húzza el a következőket kell tennie külön sorokba, használja a mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Minden elérési út egy összetett objektum megkeresése:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** A minimális séma, amely a kifejezést a táblázat összes értékének beengedi megkeresése:

    exceptions | summarize buildschema(details)

Eredmény:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Figyelje meg, hogy `indexer` szeretne megjelölni, hogy hol kell használni egy numerikus index használják. A séma néhány az elérési utak volna kell (feltételezve, hogy példa indexeknek tartomány):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Tömb és objektum literálok

Dinamikus literál létrehozásához használni `parsejson` (alias `todynamic`) egy JSON a karakterlánc-argumentum:

* `parsejson('[43, 21, 65]')`-számok tömbjével
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-számot tartalmazó dinamikus típusú egyetlen érték
* `parsejson('"21"')`-dinamikus tartalmazó karakterlánc típusú egyetlen érték

Jegyzet, hogy a JavaScript eltérően JSON arra a kettős idézőjelek (`"`) karakterláncok körül. Ezért, célszerűbb általában ajánlat JSON-címként kódolt karakterláncot literálok használatával szimpla idézőjelek (`'`).

Ez a példa létrehoz egy dinamikus, és ezután használja annak mezők:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dinamikus objektum függvények

|||
|---|---|
| *érték* `in` *tömb*| IGAZ, ha a *tömb* valamelyik elemét, amely == *érték*<br/>`where City in ('London', 'Paris', 'Rome')`
| *érték* `!in` *tömb*| IGAZ, ha a *tömb* nincs elemét, amely == *érték*
|[`arraylength(`tömb`)`](#arraylength)| Ha egy tömb nem null
|[`extractjson(`elérési út, objektum`)`](#extractjson)|Nyissa meg az objektum használja az elérési útját.
|[`parsejson(`forrás`)`](#parsejson)| Egy dinamikus objektum JSON karakterlánc változik.
|[`range(`a forrás, lépés`)`](#range)| Az értékek tömbjét
|[`mvexpand`listColumn](#mvexpand-operator) | Replikálja egy sort a minden érték a megadott cellában egy listában.
|[`summarize buildschema(`oszlop`)`](#buildschema) |Az oszlop tartalom típusa sémát infers
|[`summarize makelist(`oszlop`)`](#makelist)| Összeolvasztja sorok csoportját, és az oszlop értékeit tömb helyezi.
|[`summarize makeset(`oszlop`)`](#makeset) | Összeolvasztja sorok csoportját, és az oszlop értékeit helyezi tömb másolás nélkül.

### <a name="dynamic-objects-in-let-clauses"></a>A küldő záradékok dinamikus objektumok


[Tudathatja záradékok](#let-clause) áruházból dinamikus értékek karakterláncként, így az alábbi két záradékok között nem tesz különbséget, és a mindkettő kell a `parsejson` (vagy `todynamic`) használat előtt:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

A dinamikus tömbben lévő elemek száma.

**Szintaxis**

    arraylength(array)

**Argumentumok**

* *tömb:* A `dynamic` értéket.

**Adja eredményül.**

A *tömb*, az elemek száma vagy `null` Ha *a tömb* nem tömb.

**Példák**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Egy megadott elem ki egy elérési utat kifejezéssel JSON szöveg első. Másik lehetőségként a kibontott karakterlánc átalakítása a megadott típusú.


**Szintaxis**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Adja eredményül.**

Ez a funkció JsonPath lekérdezést az adatforrás érvényes JSON karakterláncot, attól függően, hogy a harmadik argumentum egy másik típusú tetszés szerint alakításának ezt az értéket tartalmazó hajt végre.



**Példa**

A [zárójeles] jelölés és a pont jelölések között nem tesz különbséget:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Tippek a teljesítmény**

* Hol-záradékok alkalmazása használata előtt`extractjson()`
* Fontolja meg a reguláris kifejezésekkel egyezést [bontsa ki](#extract) a használatát. Nagyon gyorsabban futtatható, és lép hatályba, ha a JSON sablonból.
* Használat `parsejson()` Ha egynél több értéket kinyerése a JSON kell.
* Érdemes a bevitel elemzi a Típus oszlop deklarálása JSON dinamikus.

### <a name="json-path-expressions"></a>A kifejezések JSON elérési út

|||
|---|---|
|`$`|Legfelső szintű objektum|
|`@`|Aktuális objektum|
|`[0]`|Tömb felső indexként|
|`.`vagy`[0]` | Gyermek|

*(Azt nem jelenleg megvalósítása helyettesítő karaktereket, rekurziót, Unió vagy szeletek.)*




### <a name="parsejson"></a>parsejson

Értelmezi egy `string` [JSON érték](http://json.org/)) és a következőképpen értéket adja eredményül `dynamic`. Célszerű kiváló használatával `extractjson()` mikor kell egynél több elemet egy összetett JSON-objektum ki kell olvasni.

**Szintaxis**

    parsejson(json)

**Argumentumok**

* *json:* A JSON-dokumentumot.

**Adja eredményül.**

Típusú objektum `dynamic` *json*által megadott.

**Példa**

A következő példában ha `context_custom_metrics` van egy `string` , hogy néz ki: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

a következő részlet olvassa be értékét, majd a `duration` tárolóhely az objektumra, és az, hogy olvassa be a két helyek, `duration.value` és  `duration.min` (`118.0` és `110.0`, illetve).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>tartomány

A `range()` függvény (nem tud összezavarodik, a `range` operátor) egy egyenletesen elosztott értéksorokban tartó dinamikus tömböt hoz létre.

**Szintaxis**

    range(start, stop, step)

**Argumentumok**

* *indítása:* Az eredményül kapott tömb első elemének értékét. 
* *leállítása:* Az eredményül kapott tömb utolsó elemének értékét, vagy a legkisebb érték, amely nagyobb, mint az utolsó elem, az eredményül kapott tömb és egész szám belül *lépés* *indítja*többszörösére.
* *lépés:* A tömb két egymást követő elem közötti különbség.

**Példák**

Az alábbi példa eredménye `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Az alábbi példa eredménye a Skype 2015 év minden nap tartó tömb:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Dinamikus értékre alakítja.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Az elérési út minden olyan kifejezés, dinamikus objektumban lévő levelek azonosító felsorolása. 

**Adja eredményül.**

Egy tömb elérési út kifejezések.

**Példák**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Megjegyzés: "[0]" jelzi a tömb jelenlétét, de nem adja meg az indexet, egy adott helyre által használt.

### <a name="zip"></a>Irányítószám

    zip(list1, list2, ...)

Egy sor olyan listák egyesíti kockára egy listáját.

* `list1...`: Értékek listája

**Példák**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Nevek

Nevek legfeljebb 1024 karakter hosszú lehet. -És nagybetűk, és a betűk, számjegyek és aláhúzás karakterek találhatók tartalmazhat (`_`). 

A név használatával ajánlat ["... '] or [" ... "] szereplő egyéb karakterek szerepeltetésére, illetve a kulcsszó használatának nevével. Példa:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["elérési út\\file\n\'x\'"] | Használat \ való escape-karakterek|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Nincs kilépés - \ konstans értéke|
|[@"\now& majd\"] | |
|[Ha] | Nyelvi kulcsszó nevével|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


