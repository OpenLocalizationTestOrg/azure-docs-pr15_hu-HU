<properties
    pageTitle="Szerkesztés típusa és a modell minőség: javaslatok API |} Microsoft Azure"
    description="Azure gépi tanulási javaslatok – rövid útmutató"
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
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Típus és a modell minőség összeállítása #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Támogatott fájltípusok ##

A javaslatok API jelenleg támogatott kétféle build: *ajánlási* és *FBT*. Minden egyes épül használatával különböző algoritmusok, és minden egyes az eltérő erősségek. A dokumentum ismerteti az egyes e buildjeiben és módszerek generált modellek minősége összehasonlítása.

Még nem tette már, azt javasoljuk, hogy befejezte az [első lépések útmutató](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Javaslat Szerkesztés típusa ###

Az ajánlási Szerkesztés típusa mátrix factorization javaslatok biztosítására használja. [Rejtett funkció](https://en.wikipedia.org/wiki/Latent_variable) módszereket ügyleteit ahhoz, mindegyik elemhez alapján hoz létre, és ezeket a rejtett módszereket segítségével összehasonlíthatja az elemek, amelyek hasonlítanak.

Ha a modellt, a elektronikai áruházban vásárlások alapján képzése, és adja meg a Lumia 650 telefon adatmodellhez bemeneteként, a a modell egy sor olyan elemeket, amelyek valószínűleg vásárolnia Lumia 650 telefonon, akik beszerzett gyakran ad eredményül. Előfordulhat, hogy az elemeket nem kiegészítő. Ebben a példában akkor lehet, hogy a modell másik telefonra ad vissza, mert a Lumia 650 kedvelő személyek lehet például másik telefonra.

Az ajánlási build vonzó legyen két funkcióját foglalja magában:

* *Támogatja az ajánlási összeállítása *hideg elem* elhelyezése **

Elemek, amelyek jelentősen használatát nem rendelkezik az úgynevezett hideg elemek. Például ha soha nem eladott előtt telefonon szállítása jelenik meg, a rendszer nem előállítani alapján a tranzakciók egyedül a termékre vonatkozó javaslatok. Ez azt jelenti, hogy a rendszer tanuljon kell-e a terméken információt.

Ha hideg elem elhelyezése használni kívánt, meg kell adnia az elemeket be a szolgáltatások adatait. Következő Ez az első néhány sor a katalógus is néz (Megjegyzés: a kulcs = érték formátum szolgáltatásokat).

>6CX-00001, a felület Pro2, Surface, írja be a hardver tároló = = 128 GB memóriát, = 4G, gyártó = Microsoft

>73H – 00013, ébresztési Xbox 360, játék,, típus = szoftver, a nyelv = angol minősítés = elért

>WAH-0F05, Minecraft Xbox 360-hoz, játék, * típus = szoftver, a nyelv spanyol, minősítés = ifjúsági =

Meg kell a következő build paraméterek beállítása:

| Paraméter összeállítása         | Jegyzetek
|------------------     |-----------
|*useFeaturesInModel*     | Állítsa be **Igaz**.  Azt jelzi, ha használható-e szolgáltatások tartalmasabbá teszi a ajánlási modell.
|*allowColdItemPlacement*   | Állítsa be **Igaz**. Azt jelzi, ha az ajánlási is leküldéses hideg elemek szolgáltatás hasonló keresztül.
| *modelingFeatureList*   | Az ajánlási tökéletesíthetik ajánlási összeállítása használandó neveinek vesszővel elválasztott listája. Például "nyelven, tároló" a fenti példa.

**Az ajánlási build támogatja a felhasználói javaslatok**

Javaslat építés [felhasználói javaslatok](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)támogatja. Ez azt jelenti, hogy a felhasználók saját tranzakció alábbi előzményeinek alapuló személyre szabott javaslatok ezáltal. Felhasználói ajánlások feltétlenül nyújt a felhasználói Azonosítójával vagy az adott felhasználó tranzakciók legutóbbi előzményeit.

Az egyik klasszikus hol lehet, hogy szeretne alkalmazni a felhasználó javaslatok példája: bejelentkezés az Üdvözöljük lapon. Nincs előléptetheti a tartalom, az adott felhasználó vonatkozik.

Is érdemes lehet alkalmazni a javaslatok build típusú, amikor a felhasználó nézze meg. Ezen a ponton a felhasználó nem megvásárolni kívánt elemek listáját van, és megadhatja a javaslatok alapján az aktuális piaci kosár.

#### <a name="recommendations-build-parameters"></a>Javaslatok paraméterek összeállítása

| név  |   Leírás |    A típus <br>  érvényes értékeket <br> (alapértelmezett érték)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   A minta hajt végre közelítések számának tükröződik a teljes számítási és a modell pontosságát. Minél nagyobb a szám, a pontosabb a modellt, de a számítási idő több időt vesz igénybe.  |   Egész szám, <br>  10-50 <br>(40)
| *NumberOfModelDimensions* |   A dimenziókat számát a modell kísérli meg az adatok belüli szolgáltatások száma vonatkozik. Méretek számának növelése lehetővé teszi a jobban a pontosabb az eredmények kisebb fürt be beállításra. Azonban túl sok méretek megakadályozza, hogy a modell elemei között összefüggések találja. |  Egész szám, <br> 10-40 <br>(20) |
| *ItemCutOffLowerBound* |  Megadja a lehető legkevesebb használatát pontok egy elemet az kell lennie ahhoz, hogy a modell részének tekinteni. |        Egész szám, <br> 2 vagy több <br> (2) |
| *ItemCutOffUpperBound* |  Megadja egy elemet az kell lennie ahhoz, hogy a modell részének tekinteni használatát pontok maximális száma. |  Egész szám, <br>2 vagy több<br> (2147483647) |
|*UserCutOffLowerBound* |   Megadja a lehető legkevesebb figyelembe kell venni a kijelzőt a modell végrehajtását követően a felhasználók tranzakciók. | Egész szám, <br> 2 vagy több <br> (2)
| *UserCutOffUpperBound* |  Megadja a tranzakciók figyelembe kell venni a kijelzőt a modell végrehajtását követően a felhasználók maximális száma. | Egész szám, <br>2 vagy több <br> (2147483647)|
| *UseFeaturesInModel* |    Azt jelzi, ha használható-e szolgáltatások tartalmasabbá teszi a ajánlási modell. |     Logikai érték<br> Alapértelmezett: igaz
|*ModelingFeatureList* |    Az ajánlási tökéletesíthetik ajánlási összeállítása használandó neveinek vesszővel elválasztott listája. A fontos funkciók függ. |    Karakterlánc, felfelé 512 karakter
| *AllowColdItemPlacement* |    Azt jelzi, ha az ajánlási is leküldéses hideg elemek szolgáltatás hasonló keresztül. | Logikai érték <br> Alapértelmezett: False
| *EnableFeatureCorrelation*    | Azt jelzi, ha a használható funkciók érvelés. | Logikai érték <br> Alapértelmezett: False
| *ReasoningFeatureList* |  A mondatok, például ajánlási magyarázatok mintafelismerési használandó neveinek vesszővel elválasztott listája. A felhasználók fontos funkciók függ. | Karakterlánc, felfelé 512 karakter
| *EnableU2I* | Engedélyezze a személyre szabott javaslatok, más néven felhasználó a javaslatok (U2I) elemet. | Logikai érték <br>Alapértelmezett: igaz
|*EnableModelingInsights* | Megadja, hogy offline kiértékelés gyűjtése modellezési háttérismeretek (Ez azt jelenti, hogy a pontosság és sokszínűség mértékek) hajtható végre. Ha igaz, az adatok egy részhalmazát nem fogja használni a képzési, mert szüksége lesz kíván fenntartani a modell tesztelése. További információ [a kapcsolat nélküli értékelést](#OfflineEvaluation). | Logikai érték <br> Alapértelmezett: False
| *SplitterStrategy* | Háttérismeretek modellezése engedélyezése értéke *Igaz*, ha az hogyan adatokat kell osztani a kipróbálás céljából.  | Karakterlánc, *RandomSplitter* vagy *LastEventSplitter* a <br>Alapértelmezett: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>FBT Szerkesztés típusa ###

Gyakran vásárolt együtt (FBT) összeállítása tartalmaz, amely a két vagy három különböző termékek közös fordulhat elő, közös időpontok darabszáma elemzését. Kattintson a halmaz egy hasonló függvényekkel ( **emelje fel a****közös előfordulások**, **Jaccard**,) rendezése történik.

Tekintsen úgy **Jaccard** és **emelje fel** , mint a közös előfordulások normalizálása módjai.  Ez azt jelenti, hogy az elemek csak akkor, ha visszatér azok Ha vásárolt együtt az seed (mag) elemet.

Példánkban Lumia 650 phone telefonon X visszatér csak akkor, ha a telefon X vásárolta meg az adott munkamenetben, mint a Lumia 650 telefonon. Valószínűleg nem lehet, mert azt az elvárt elemek kiegészítő, a Lumia 650 vissza kell; a képernyő védő, vagy például egy power adaptert a Lumia 650.

Két elem jelenleg tekinti vannak vásárolt az adott munkamenetben, ha az azonos Felhasználóazonosítóját és időbélyeg tranzakció előfordulási helyük.

FBT buildjeiben nem támogatják a hideg elemek, mert a meghatározás által elvárt két elemek ugyanazon tranzakció lehet megvásárolni. FBT buildjeiben elemkészletek (triplets) térhet vissza, amíg nem támogatják a személyre szabott javaslatok mivel elfogadják, mint a bemeneti egy egyetlen seed (mag) elemre.


#### <a name="fbt-build-parameters"></a>FBT build paraméterei

| név  |   Leírás |       A típus  <br> érvényes értékeket <br> (alapértelmezett érték)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Hogyan óvatos a modell található. Elemek modellezési veendő közös előfordulásainak száma. |  Egész szám, <br> 3 – 50 <br> (6)
| *FbtMaxItemSetSize* | A gyakori készlet elemszámát Bounds.| Egész szám  <br> 2 – 3 <br> (2)
| *FbtMinimalScore* | Gyakori meg kell rendelkeznie a visszaadott találatok szerepeltetni minimális pontszámhoz. Minél nagyobb a jobban. | Dupla <br> 0 vagy újabb <br> (0)
| *FbtSimilarityFunction* | Határozza meg, hogy a hasonló függvény összeállítása való használatra. **Emelje fel** részesítése szemben serendipity, **közös előfordulás** előreláthatóság részesítése szemben, és **Jaccard** egy biztonságos a kettő között. | Karakterlánc,  <br>  <i>cooccurrence, felvonó, jaccard</i><br> Alapértelmezés: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Szerkesztés értékelése és kiválasztása ##

Ez az útmutató előfordulhat, hogy könnyebben eldöntheti, hogy e javaslatok építés vagy egy FBT build kell használnia, de nem adja meg a végleges válasz azokban az esetekben, ahol használhatja is lehet őket. Is még akkor is, ha tudja, hogy egy FBT build típusú használni kívánt, továbbra is érdemes lehet **Jaccard** vagy **emelje fel** a hasonló függvényében kiválasztását.

A legjobb módszer két különféle buildjeiben között jelölje ki őket teljesítménynövekedés (online értékelést) megvizsgálja, és nyomon követheti a különféle buildjeiben számára átváltási díjának. A konvertálás ráta ajánlási kattintással alapján lehet mérni, a tényleges száma a javaslatok látható, vagy akár a tényleges vásárlás összegek vásárol, amikor a különböző javaslatokat volt látható. A konvertálás ráta metrikus alapján az üzleti célt, kijelölheti azokat.

Egyes esetekben érdemes felmérése offline a modell, mielőtt gyártási helyezi. Míg offline kiértékelés nem helyettesítheti a online kiértékelési, azt egy mérőszám szolgálhatnak.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Kapcsolat nélkül végzett kiértékelésre  ##

Kapcsolat nélküli értékelést célja pontosság (a felhasználókat, vásárolhat egyet az ajánlott elemek száma) és a javaslatok (ajánlott elemek száma) skálája előrejelzésére.
A pontosság és sokszínűség mértékek kiértékelés részeként a rendszer a felhasználók minta talál és kettéválaszthatja-e a tranzakciókat, valamint ezen felhasználók két csoportokba: az oktatás adatkészlet és a vizsgálat adatkészlet.

> [AZURE.NOTE]Kapcsolat nélküli mértékek használatához rendelkeznie a használati adatok időbélyegeket.
> Időadatok szükség felosztása megfelelően közötti képzés és próba adatkészleteket használatát.

> Kapcsolat nélküli kiértékelés is, nem eredményezhet kis használatát fájlok eredmények. A kiértékelés alapos, hogy a próba adathalmazban 1000 használatát pontjaira legalább kell lennie.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Pontosság a k ###
Az alábbi táblázat a a pontosság, k – offline kiértékelésének eredménye jelöli.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Százalékos |   13.75 | 18.04   | 21 |  24.31 | 26.61
|Próba-felhasználók |    10 000 |    10 000 |    10 000 |    10 000 |    10 000
|A felhasználók tekinteni | 10 000 |    10 000 |    10 000 |    10 000 |    10 000
|A felhasználók nem veszi figyelembe az | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
A fenti táblázatban *k* helyén javaslatok az ügyfélnek látható számát. A táblázat az alábbihoz hasonló: "A próba időszakban csak egy ajánlási volt látható az ügyfelekkel, ha a felhasználók csak 13.75 volna vásárolt, hogy ajánlási." Ez az utasítás feltételezve, hogy a modell vásárlás adatokkal lett-e képzett alapul. Úgy is fel, hogy ez érték, hogy a pontosság: 1 13.75.

Megfigyelheti, hogy több elemet az ügyfélnek jelennek meg, mint annak valószínűségét, hogy az ügyfél vásárlásával egy javasolt elemre mutat. Az előző kísérletet a annak valószínűségét számítja ki szinte megduplázódik 26.61 százalék esetén ajánlott 5 elemet.

#### <a name="percentage"></a>Százalékos
A felhasználóknál, akik legalább egy, a *k* ajánlások kezelni százalékát jelenik meg. A százalékos érték kiszámítása tekintett felhasználók száma szerint legalább egy ajánlási kezelni felhasználók számát. Lásd: felhasználók tekintett további információt.

#### <a name="users-in-test"></a>Próba-felhasználók
Ez a sor adatait a próba adatkészlet felhasználók száma jelöli.

#### <a name="users-considered"></a>A felhasználók tekinteni
A felhasználó csak tekintendő, ha a rendszer a modellt, az oktatás adatkészlet használatával létrehozott alapján *k* elemek legalább ajánlott.

#### <a name="users-not-considered"></a>A felhasználók nem veszi figyelembe az
Ez a sor adatait helyén azok a felhasználók nem veszi figyelembe. A felhasználók, amely nem kapta legalább *k* ajánlott elemek.

A felhasználó nem veszi figyelembe az = teszt – a felhasználók felhasználók figyelembe venni.

<a name="Diversity"></a>
### <a name="diversity"></a>Sokszínűség ###
Sokszínűség mértékek mérjük ajánlott elemekre. Az alábbi táblázat a a sokszínűség offline kiértékelésének eredménye jelöli.

|PERCENTILIS gyűjtő |    0-90|  90 – 99| 99-100
|------------------|--------|-------|---------
|Százalék        | 34.258 | 55.127| 10.615


Ajánlott elemek teljes: 100 000

Ajánlott egyedi elemek: 954

#### <a name="percentile-buckets"></a>PERCENTILIS időszakok
Minden egyes PERCENTILIS gyűjtő jelöli a módosítva (minimális és maximális értékek, amelyek a 0 és 100 között). Az elemek közelébe 100 a legnépszerűbb elemek, illetve a cikkek közelébe 0 népszerű legalább. Például ha a százalékos értékét a 99-100 PERCENTILIS gyűjtő 10.6, az azt jelenti, hogy a javaslatok 10.6 százaléka az eredmény a felső egy százalék a legnépszerűbb elemek. A PERCENTILIS gyűjtő minimális érték végpontokat is beleértve, és a kizárólagos, kivéve a 100 maximális értéke.
#### <a name="unique-items-recommended"></a>Ajánlott egyedi elemek
Az egyedi elemek metrikus ajánlott értékeléséhez visszaküldött különböző elemek számát mutatja.
#### <a name="total-items-recommended"></a>Ajánlott elemek száma
A teljes elemek ajánlott metrikus ajánlott elemek számát mutatja. Néhány lehet az ismétlődő elemeket.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Kapcsolat nélküli kiértékelés mérőszámok ###
A pontosság és sokszínűség offline mérési módja miatt akkor lehet hasznos, mely build használandó kiválasztásakor. Szerkesztés egyszerre a megfelelő FBT vagy ajánlási részeként összeállítása a Paraméterek:

-   Állítsa a *enableModelingInsights* build paraméter **Igaz**.
-   Ha szükséges, jelölje be a *splitterStrategy* (vagy *RandomSplitter* vagy *LastEventSplitter*).
*RandomSplitter* kettéválaszthatja-e a használati adatok vonaton és próba eredménye a megadott *randomSplitterParameters* próba készültségi alapján és véletlen rendező értékeket.
*LastEventSplitter* kettéválaszthatja-e a használati adatok vonaton és tesztelje a beállítja az egyes felhasználókhoz az utolsó tranzakció alapján.

Ez elindítja a építés, használja az adatok csak egy részhalmazát – oktatás, valamint a többi adatot kiértékelés mértékek számítja ki.  A Szerkesztés befejezése után a kimenet a kiértékelés, hogy meg kell fordulnia a [Get-összeállítás mértékek API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), a megfelelő *modelId* és *buildId*.

 Az alábbiakban a minta kiértékelés JSON kimenetét.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
