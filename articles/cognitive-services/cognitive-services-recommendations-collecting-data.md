<properties
    pageTitle="A modell betanítása adatgyűjtés: a gép tanulási javaslatok API |} Microsoft Azure"
    description="Azure gépi tanulási javaslatok - adatgyűjtés a modell betanítása"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Adatgyűjtés a modell betanítása #

A javaslatok API folyamatosan tanul elmúlt ügyleteit mi legyen az elemek keresése a javasolt, hogy egy felhasználó.

Miután létrehozott egy adatmodellt, meg kell adnia a két valamilyen információra, mielőtt képzést, végezheti el: a katalógus fájlt, és használati adatainak.

>   Ha, még nem tette már, javasoljuk, hogy az [első lépések útmutató](cognitive-services-recommendations-quick-start.md)elvégzéséhez.


## <a name="catalog-data"></a>Katalógusadatok ##

### <a name="catalog-file-format"></a>Katalógus fájlformátum ###

A katalógus fájlt, az ügyfélnek vannak kínálnak cikkekre vonatkozó információkat tartalmazza.
A katalógusadatok kell hajtsa végre a következő formátumban:

- Szolgáltatások - nélkül`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- -Szolgáltatásokkal`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Minta sorok katalógus fájlban

Az alábbi szolgáltatások: nélkül

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Az alábbi szolgáltatások:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>A formátum részletei

| név | Kötelező | Típus |  Leírás |
|:---|:---|:---|:---|
| Listaelem-azonosító |igen | [A-z], [a-z], [0 – 9-es], [_] & #40. Aláhúzás és #41; [-] & #40; kötőjel & #41;<br> A maximális hossza: 50 | Egy elem egyedi azonosítója. |
| Elem neve | igen | Tetszőleges alfanumerikus karakter<br> A maximális hossza: 255 | Elem neve. |
| Elem kategória | igen | Tetszőleges alfanumerikus karakter <br> A maximális hossza: 255 | Kategóriájára ezt az elemet (például főző könyvek, ábráknak...); üres is lehet. |
| Leírás | Nem, kivéve, ha a funkciók bemutatása (de üres is lehet) | Tetszőleges alfanumerikus karakter <br> A maximális hossza: 4000 | Ez a cikk leírását. |
| A szolgáltatások listában | nem | Tetszőleges alfanumerikus karakter <br> A maximális hossza: 4000; Szolgáltatások: 20 maximális száma | Funkció neve vesszővel tagolt listáját = tartalmasabbá teszi a modell ajánlási használható funkció érték.|

#### <a name="uploading-a-catalog-file"></a>Katalógus fájl feltöltésekor

Nézze meg az [API-hivatkozás](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) : a katalógus fájl feltöltésekor.  

Figyelje meg, hogy a katalógus fájl tartalmát a kérelem szövegeként adható át.

Ha több katalógus fájl feltöltése a több hívások ugyanazt a modellt, azt az új alkalmazáskatalógus elemek szúrja be. Az eredeti értékeket tartalmazó elemek marad. Katalógusadatok segítségével, ezzel a módszerrel nem tudja frissíteni.

>   Megjegyzés: A maximális fájlmérete 200 MB-ot.
>   A támogatott katalógus elemek maximális száma 100 000 elem.


## <a name="why-add-features-to-the-catalog"></a>Miért szolgáltatások hozzáadása a katalógus?

A javaslatok motor statisztikai modell, amely közli, hogy milyen elemeket valószínűleg kedvelte, vagy egy ügyfél által megvásárolt hoz létre. Ha egy új termék vele soha nem kezelni, nincs lehetőség a modell létrehozni, ha a közös előfordulások egyedül. Tegyük fel a kezdés felajánló új "gyerekek violin" az áruházban, mivel soha nem eladott adott violin előtt nem tudja megállapítani, hogy az adott violin javasoljuk, hogy milyen egyéb elemek.

Említett, ha a motor tudja, hogy az adott violin információt (tehát még egy musical instrument, a gyermekek életkor 7 – 10, nem egy drága violin, stb.), majd a motor talál a többi terméktől, a hasonló funkciókat. Például violin a múltbeli eladott, és általában, hogy az hegedűket vásárlása gyakran klasszikus zenei CD-re és lap zenét jelző vásárolni.  A rendszer ezek a szolgáltatások közötti kapcsolatok keresheti meg és javaslatok alapján a szolgáltatásokat, míg az új violin kis használatát adja meg.

Funkciók a program importálja a katalógusadatok részeként, és kattintson a rangját (vagy a szolgáltatás a modell fontosságát) kapcsolódik egy sorrendet megadó Szerkesztés befejezése. Szolgáltatás rang a mintázat típusú elemeket és használati adatainak szerint módosíthatja. De egységes használatát elemek, rangját csak kisebb ingadozásaira vannak. Szolgáltatások rangját nem negatív szám. A 0 értéket, az azt jelenti, hogy a szolgáltatás nem lett rangsorban (történik, ha, ez az első sorszám összeállítása végrehajtására előtt API hívása). A dátumot, amelynél rangját hozzárendelésre lett pontszámhoz frissessége neve.


###<a name="features-are-categorical"></a>Szolgáltatásai Categorical

Ez azt jelenti, hogy funkciók, amelyek hasonlítanak a kategória kell létrehoznia. Ár például = 9.34 nem kockák funkciót. A szolgáltatás azonban, például az priceRange = Under5Dollars lehetővé teszi a kockák. Egy másik hiba környezetbe az elem neve szolgáltatásként. Ez okozhatja elem egyedinek kell lennie, azt nem le kategória nevére. Ellenőrizze, hogy a funkciók kategóriák elemeket jelenítik meg.


###<a name="how-manywhich-features-should-i-use"></a>Hogyan sok vagy amelybe szolgáltatások érdemes használnom?


Végül a javaslatok összeállítása épület adatmodell legfeljebb 20 funkcióival támogatja. Tudta hozzárendeli több, mint 20 funkciók az elemeket a katalógus, de várhatóan végezze el a rangsorolási fejlesztése, és válassza a csak a szolgáltatások magas, hogy hányadik. (Egy funkció mellett egy rangját 2.0-s vagy több olyan valójában jól használható szolgáltatás használata!). 


###<a name="when-are-features-actually-used"></a>Mikor kell szolgáltatások ténylegesen használni?

Ha nem elég tranzakció adatok javaslatok egyedül tranzakció információt biztosít a modell funkciók használják. Szolgáltatások így a lehető leglátványosabb hatást a "hideg elemek" – néhány tranzakciók lesz. Ha minden elem van elegendő tranzakció információkat, előfordulhat, hogy nem kell a modell funkcióival kiegészítése.


###<a name="using-product-features"></a>A termék szolgáltatásainak használata

A Szerkesztés kell részét képező szolgáltatások használatának:

1. Ellenőrizze, hogy a katalógus szolgáltatásokkal rendelkezik a töltse fel.

2. Rangsorolási építés elindítani. Ez hajt végre az elemzés a sürgős/rangját funkciói.

3. Javaslatok építés elindítása, a paraméterek beállítása a következő összeállítása: igaz, igaz, allowColdItemPlacement useFeaturesInModel beállítása és modelingFeatureList tartalmasabbá teszi a modell használni kívánt szolgáltatások vesszővel elválasztott listája értékre kell állítani. További információt a [javaslatok összeállítása paramétereket](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) talál.





## <a name="usage-data"></a>Látogatottsági adatok ##
Használatát fájl ezeket az elemeket használata vagy a tranzakciók az üzleti adatokat tartalmazza.

#### <a name="usage-format-details"></a>Használatát a formátum részletei
Egy fájl használatát egy CSV (pontosvesszővel elválasztott érték-)-fájlt, ahol használatát fájl minden egyes sorához jelöli a felhasználó és az elemek közötti kapcsolat. Minden egyes sorára formázása az alábbi képlettel történik:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| név  | Kötelező | Típus | Leírás
|-------|------------|------|---------------
|Felhasználói azonosító|         igen|[A-z], [a-z], [0 – 9-es], [_] & #40. Aláhúzás és #41; [-] & #40; kötőjel & #41;<br> A maximális hossza: 255 |Egy felhasználó egyedi azonosítója.
|Listaelem-azonosító|igen|[A-z], [a-z], [0 – 9-es], [& #95;] & #40. Aláhúzás és #41; [-] & #40; kötőjel & #41;<br> A maximális hossza: 50|Egy elem egyedi azonosítója.
|Idő|igen|Formátumú dátum: YYYY/hh/ddTHH (pl. 2013/06/20T10:00:00)|Az adatok idő.
|Esemény|nem | Az alábbiak egyikét:<br>• Kattintás<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Megvásárlása| A tranzakciók típusát. |

#### <a name="sample-rows-in-a-usage-file"></a>Minta sorok használatát a fájlon

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Használatát fájl feltöltésekor

Nézze meg a [API hivatkozás](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) használatát fájlfeltöltéskor.
Fontos tudni, hogy Ön a tartalom használatát fájl átadni a HTTP-hívás szövegtörzseként.

>  Megjegyzés:

>  Maximális méret: 200 MB-ot. Több használatát fájl előfordulhat, hogy töltse fel.

>  Katalógus fájl feltöltése, mielőtt megkezdené a használati adatok felvétele a modellbe szüksége. Csak a katalógus fájlban lévő elemeket a oktatás fázisban történik. Minden elem figyelmen kívül hagyja.

## <a name="how-much-data-do-you-need"></a>Hogy mennyi adatot van szüksége?

A modell minőségének érték erősen minőségének és az adatok mennyiségét függ.
A rendszer folyamatosan tanul felhasználók vásárolt sokféle elemhez (hívása a közös előfordulások). FBT buildjeiben célszerű is fontos tudni, hogy mely elemek vásárolt ugyanazt a tranzakciók. 

Egy jó a legjobb megoldás, hogy az elemek többségében 20 tranzakciók vagy több, lehet, ha a katalógus 10 000 elem, volna javasoljuk, hogy rendelkezik-e legalább 20 alkalommal adott számú tranzakciók vagy körülbelül 200,000 tranzakciók. Még egyszer az egy szabály a legjobb megoldás. Szüksége lesz az adatok kísérletezhet.

Miután létrehozott egy adatmodellt, végezheti el egy [offline kiértékelés](cognitive-services-recommendations-buildtypes.md) ellenőrizni, hogy mennyire a modell valószínűleg a végrehajtásához.
