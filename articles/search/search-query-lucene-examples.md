<properties
    pageTitle="Azure keresési lekérdezés példák Lucene |} Microsoft Azure-keresés"
    description="Lucene lekérdezés-szintaxis zavaros keresési, az közelség keresési, a kifejezés szolgáló lehetőségekről, a reguláris kifejezésekkel keresés és a keresés helyettesítő karakterekkel."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene lekérdezési szintaxis példák Azure keresési lekérdezések készítéséhez

Azure keresési lekérdezések kiszámításakor, vagy az alapértelmezett [Egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/azure/dn798920.aspx) , vagy az alternatív [Lucene lekérdezés elemző Azure keresés](https://msdn.microsoft.com/library/azure/mt589323.aspx)is használhatja. Az Lucene lekérdezés elemző konstrukciók lekérdezés összetettebb, például a lekérdezések mező – munkafüzetszintű, zavaros keresési, közelség keresési, kifejezés szolgáló lehetőségekről és reqular kifejezés keresési támogatja.

Ez a cikk is lépéseinek példák, amelyek Lucene lekérdezés-szintaxis és az eredmények egymás melletti megjelenítése. Példa egy előre elkészített keresési indexet [JSFiddle](https://jsfiddle.net/), egy online kód editor tesztelés parancsfájl és HTML futtassanak. 

Kattintson a jobb gombbal a lekérdezés URL-példákat JSFiddle megnyitása külön böngészőablakban.

> [AZURE.NOTE] Az alábbi példák kihasználhatja a keresési index álló feladatok érhető el a [Város a New York OpenData](https://nycopendata.socrata.com/) kezdeményezés által biztosított adatkészletet alapján. Az adatok nem tekinthető aktuális vagy a teljes. Az index van a Microsoft által nyújtott védőfalas szolgáltatás. Nem kell az Azure előfizetést vagy Azure keresési próbálja ki ezeket a lekérdezéseket.

## <a name="viewing-the-examples-in-this-article"></a>Ez a cikk a Példák megtekintése

Ez a cikk példáiban összes adja meg, hogy az Lucene lekérdezés elemző a**queryType** keresési paraméter keresztül. Amikor az Lucene lekérdezés elemző át a kódot, minden kérésre fogja adja meg a a **queryType** .  Az érvényes értékek **egyszerű**|**teljes**, az **egyszerű** az alapértelmezett és a **teljes** a Lucene lekérdezés elemző. Lásd: [Dokumentumok keresése (Azure keresési szolgáltatás REST API -val)](https://msdn.microsoft.com/library/azure/dn798927.aspx) a lekérdezés paraméterei megadásával kapcsolatban további tájékoztatást.

**Példa 1** – kattintson a jobb gombbal a következő lekérdezés kódtöredékének kattintva nyissa meg az új böngésző lap JSFiddle betöltő, és futtatja a lekérdezést:
- [& queryType = teljes és a keresés = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

A lekérdezés dokumentumok a feladatok indexben (a védőfalas szolgáltatás betöltve) adja eredményül.

Az új böngészőablakban látni fogja a a JavaScript forrás- és HTML kimenet egymás mellett. A parancsprogram egy lekérdezés, amelyet a példa URL-címeit, a jelen cikkben által biztosított hivatkozik. Például az **Példa 1**, a mögöttes lekérdezést akkor az alábbi képlettel történik:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Értesítés a lekérdezés előre beállított Azure keresési index létrehozása című nycjobs használja. A **searchFields** paraméter korlátozza a keresés csak a vállalati cím mezőben. A **queryType** értéke **teljes**, amely arra utasítja az Azure-keresés a Lucene lekérdezés elemzővel használni ezt a lekérdezést.

### <a name="fielded-query-operation"></a>Fielded lekérdezési művelet

Ez a cikk példáiban módosíthatja egy **fieldname:searchterm** építés fielded lekérdezés művelet, ahol a mező egy adott szót, és a keresőkifejezést is egy szót vagy kifejezést, tetszés szerint a logikai operátorokat meghatározása a megadásával. Többek között az alábbiakat:

- business_title:(senior NOT junior)
- állapot: ("Siófok" és "Új Jersey")

Győződjön meg arról, helyezze idézőjelek között több karakterláncok, ha azt szeretné, hogy mindkét ahogy ebben az esetben a hely mezőben két különböző városok keresése egy egységként ki kell értékelni karakterláncokat. Arról is az az operátor nagy kezdőbetűvel, nem fogja látni, és and

A mezőben megadott **fieldname:searchterm** kereshető mezőnek kell lennie. Index attribútumok használata a mező definíciók kapcsolatos részletekért lásd: a [Create Index (Azure keresési szolgáltatás REST API -val)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Intelligens keresés

A zavaros keresési találatok megtalálja az egy hasonló építés kifejezéseket. Egy [Lucene dokumentáció](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)zavaros keresések alapuló [Damerau-Levenshtein távolság](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

A zavaros keresési használja a tilde "~" szimbólum választható paraméter, megadó érték 0 és a 2, közötti távolság Szerkesztés egyetlen szó végén. Ha például "kék ~" vagy "kék ~ 1 cm-es adja vissza kék, kékek és kapcsolás.

**Példa 2** – kattintson jobb gombbal a következő lekérdezés kódtöredékének tegyen egy próbát. A szerződési időszak vezető őket, de nem kezdő üzleti adatfeliratokat keresi meg ezt a lekérdezést:

- [& queryType = teljes és a keresés nem business_title:senior = kezdő](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Közelség keresés

Közelség keresések segítségével keresse meg a kifejezések, amelyek egymás mellett a dokumentumban. A tilde beszúrása "~" kifejezés végén szimbólum követ, amely a közelség oszlopazonosító létrehozása szavak számát. Ha például "szállások airport" ~ 5 kifejezésre keresve a kifejezések szállások és minden más 5 szavak airport dokumentum.

**Példa 3** – kattintson a jobb gombbal a következő lekérdezés kódtöredékének. A lekérdezés keresi meg a feladatot a kifejezés társítása (ahol azt elírás van) együtt:

- [& queryType = teljes és a keresés = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Példa 4** – kattintson a jobb gombbal a lekérdezésre. A "vezető elemző" kifejezést a feladatok keresése hol elválasztott legfeljebb egy szóval:

- [& queryType = teljes és a keresés = business_title: "vezető elemző" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Példa 5** – próbálja ki ismét eltávolítása a szavak közötti "vezető elemző" kifejezés.

- [& queryType = teljes és a keresés = business_title: "vezető elemző" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Távon teljesítményfokozó

Nagyobb, ha a boosted kifejezést, a kifejezés nem tartalmazó dokumentumok viszonyított tartalmaz dokumentum rangsorolási kifejezés szolgáló lehetőségekről hivatkozik. Ez különbözik pontozási profilokat, annak, hogy a pontozási profilok növelése leírják helyett bizonyos mezőket. A következő példa segítséget nyújt a különbséget szemléltetik.

Fontolja meg egy pontozási profillal, amely növekedhet megfelel egy bizonyos mezőben, például **Műfaj** musicstoreindex példában. Kifejezés szolgáló lehetőségekről használhatóak további növelése bizonyos keresési feltételek nagyobb, mint másoknak. Ha például "rock ^ 2 elektronikus" fog növeli a nagyobb, mint a többi kereshető mezőt a tárgymutató **Műfaj** mezőben a keresési kifejezések tartalmazó dokumentumok. Ezenkívül a keresőkifejezést "rock" tartalmazó dokumentumok fog kell rangsorban nagyobb, mint a többi "elektronikus" kifejezés erősítése értékét (2) eredményeként keresőkifejezést.

Kifejezés növelése, használja a beszúrási jelet "^", szimbólum erősítése tényezővel (szám) végén található a keresett kifejezést. Minél nagyobb erősítése tényező, a fontosabb a kifejezés más keresési kifejezések viszonyított lesz. Alapértelmezés szerint a erősítése tényező érték 1. Bár a erősítése tényező pozitív kell lennie, ez lehet az 1-nél kisebb (például 0,2).

**Példa 6** – kattintson a jobb gombbal a lekérdezésre. Keresse meg a kifejezés "számítógép elemző" hol vannak a szavak számítógép és a elemző találatok nélkül, de elemző feladatok vannak az eredmények tetején láthatja a feladatokat.

- [& queryType = teljes és a keresés = business_title:computer elemző](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Példa 7** – próbálja meg újra, ez idő szolgáló lehetőségekről eredmények a szerződési időszak számítógéppel fölé a szerződési időszak elemző két szavak nem léteznek.

- [& queryType = teljes és a keresés = business_title:computer ^ 2 elemző](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Reguláris kifejezések

Reguláris kifejezések keresés egyezést közötti perjel "/", mint dokumentált [RegExp szolgáltatást osztály](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html)tartalma alapján.

**Példa 8** – kattintson a jobb gombbal a lekérdezésre. Keresse meg vagy feladatok vezető vagy Alsós kifejezés.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Ez a példa URL-címe lesz nem jelenik meg megfelelően a lapon. Kerülő másolja az alábbi URL-CÍMÉT, és illessze be a böngésző URL-címe:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Keresés helyettesítő karakterekkel

Több általában felismert szintaxisa is használhatja (\*) vagy egyszeri (?) karaktert helyettesítő karaktereket. Megjegyzés: a Lucene lekérdezés elemzővel egyetlen kifejezés és a kifejezést nem a szimbólumok használatát támogatja.

**Példa 9** – kattintson a jobb gombbal a lekérdezésre. Keressen rá az előtag "program", amely tartalmaz üzleti címek programozási kifejezéseket és programozója azt tartalmazó feladatokat.

- [& queryType = a teljes & $select = business_title & keresési = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Nem használhatja a * vagy? szimbólum, mint az első karakter keresés.


## <a name="next-steps"></a>Következő lépések

Adja meg a Lucene lekérdezés elemzővel az a kódot. Az alábbi hivatkozások bemutatják, hogy miként állíthatja be a keresési lekérdezések .NET és a REST API-t is. A hivatkozások a szintaxissal alapértelmezett egyszerű, meg kell alkalmazni, ez a cikk a **queryType**megadását tanultak.

- [A lekérdezés a Azure keresési Index a .NET SDK használatával](search-query-dotnet.md)
- [A lekérdezés a Azure keresési Index a REST API](search-query-rest-api.md)


 