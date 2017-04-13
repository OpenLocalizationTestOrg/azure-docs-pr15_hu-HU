<properties
   pageTitle="Az egyszerű modell – regressziós modell választ előrejelzésére |} Microsoft Azure"
   description="Hogyan lehet egy egyszerű regressziós modellt, az adatok tudományos videó 4 kezdőknek ár előrejelzésére. Egy lineáris regressziós cél adatokkal tartalmazza."                                  
   keywords="létrehozása modell, egyszerű modell, ár előrejelzési, egyszerű regressziós modell"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Az egyszerű modell választ előrejelzésére

## <a name="video-4-data-science-for-beginners-series"></a>Videó: 4: Adatok tudományos kezdők adatsor

Megtudhatja, hogyan hozhat létre egy egyszerű regressziós modell előre, az adatok tudományos videó 4 kezdőknek rombusz árát. Azt fogja rajzolja meg a regressziós modell cél adatokkal.

A hatékony kihasználása a sorozat, nézze meg azokat az összes. [Nyissa meg a videók listája](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>További videók a sorozat

*Adatok tudományos kezdőknek* az öt a rövid videókból adatok tudományos rövid útmutatást talál.

  * 1 a videó: [Adatok tudományos 5 kérdésekre választ](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*
  * Videó: 2: [adatok tudományos készen áll az adatok?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 perc 56 másodperc)*
  * Videó: 3: [Az adatok fogadásához kérdése van](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 perc 17 másodperc)*
  * Videó 4: Egyszerű modell választ előrejelzésére
  * 5 a videó: [Mások munka végezze el az adatok tudományos másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Átirat: Egyszerű modell választ előrejelzésére

Üdvözli a negyedik videót a "adatok tudományos az kezdők" a sorozat. Ezt az egy azt fogja egyszerű modell összeállítása, és ellenőrizze a szövegbevitel.

A *modell* az adatokkal kapcsolatos egyszerűsített írás. E kiderül, hogy e jelenti.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Vegyük fel vonatkozó, a pontos, a csatlakoztatott, elegendő adat

Tegyük fel, rombusz szeretnék szalon. A NO@LOC 1.35 beszúrási rombusz az egy beállítás tartozott, gyűrű-e, és arról, hogy mennyibe fog kerülni első kívánt. Lehet a Jegyzettömb és a toll a ékszereket tárba, és az ár, minden esetben és azok karát mennyire összehasonlítani a rombuszokká lejegyezni lehet. Az első rombusz – annak 1.01 karát és $7,366 kezdve.

Most már hajtsa végre, és tegye ezt a tárolóban lévő minden más rombuszokká.

![Adatoszlopok rombusz](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Figyelje meg, hogy a lista két oszlop szerepel. Minden egyes oszlopát különböző attribútummal rendelkezik - a vastagság karát és ár -, illetve minden egyes sor egy adatpontra, amely egyetlen rombusz.

Ténylegesen már létrehozott egy kis adatkészlet-– az alábbi táblázat. Figyelje meg, hogy megfelel-e a feltétel a minőség:

* Az adatok **vonatkozó** - súly növekedni biztosan kapcsolódó ár
* **Pontos** - azt double-checked azt írja le árak
* **Csatlakoztatva** – ne legyenek ezek az oszlopok egyikével üres szóközök
* És azt megjelenik, **elég** a kérdés adatok

## <a name="ask-a-sharp-question"></a>Éles kérdés feltevése

Most azt fogja jelentenek a kérdés éles útján: "mennyibe fog kerülni 1.35 beszúrási rombusz vásárlása?"

A lista nem tartalmaz 1.35 beszúrási rombusz, hogy be kell az adatokat a többi választ a kérdésére használata.

## <a name="plot-the-existing-data"></a>A meglévő adatok ábrázolása

Először is azt fog sikerülni lesz, rajzolása vízszintes szám, a diagram a súlyok tengelyt nevű. A súlyok tartománya 0-2, így azt fogja vonal rajzolása, amely bemutatja, hogy a tartományt, és helyezze az egyes fele beszúrási osztások.

Ezután azt fogja rajzolja meg a függőleges tengely rögzítheti az ár, és csatlakoztassa a vastagság vízszintes tengely. Ez az erőforrás-mennyiség dolláros lesz. Most egy sor olyan koordinálása tengelyek van.

![Súly és ár tengelyek](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Most már készíthet a ezeket az adatokat, és kapcsolja be *ábrázolása pont*megyünk. Az numerikus adatkészletek ábrázolása kiválóan használhatók.

Az első adatpont 1.01 karát a függőleges vonal eyeball azt. Ezután a $7,366 vízszintes vonal eyeball azt. Felel meg, ha azt rajzolja meg a pont. Az első rombusz jelöl ki.

Most azt a lista egyes rombusz folyamatát, és ugyanezt a célt szolgálja. Sajnos keresztül, amikor ez azt első: kitöltenem, az pont, az egyes rombusz.

![Pontdiagram ábra](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>A modell a adatpontokra rajzolása

Ha megnézi pontok és squint, a webhelycsoport ekkor ilyen fat, zavaros vonal. Hogy a jelölőt készítése és keresztül egyenes vonal rajzolása.

A vonal rajzolása által létrehozott egy *adatmodellt*. Tekintsen úgy, mint teljesítménynövekedés írása, és készít egy simplistic rajzos változatát. Most a rajzos történt - a sor nem lépjen az adatpontok keresztül. De, a hasznos egyszerűsítése.

![Lineáris regressziós egyenes](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

A FAKT, hogy az összes pontot nem pontosan a sor az OK gombra. Adatok tudósok ez ismertetik, amely arról tájékoztat, hogy nincs a modell –, amely a sor - és majd minden pont néhány *zajt* vagy *eltérése* társítva. Az alapul szolgáló tökéletes kapcsolatot, és ezután van a kövecses, valós világ zajt és bizonytalanság hozzáadó.

Mivel a kérdés próbálja azt *mennyi?* egy *regresszió*Link. És egy *lineáris regressziós*egyenes programmal mutatjuk be, mert.

## <a name="use-the-model-to-find-the-answer"></a>A modell segítségével találja a választ kérdésére

Most már van egy adatmodellt, és a kérdés kérjük,: fog 1.35 beszúrási rombusz mennyibe?

A kérdés megválaszolásához, hogy szem 1.35 karát, és függőleges vonal rajzolása. Azt a modell sor metszéspontjának azt eyeball a forint tengely vízszintes vonallal. Azt a találatok száma 10 000 elérhető. Gémhez! Ez a válasz: 1.35 beszúrási rombusz költségek 10 000 $ készül.

![Keresse meg a választ a modell](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Hozzon létre egy konfidencia-intervallum átállítása

Fontos, hogy hogyan pontos a szövegbevitel nem wonder természetes. Hasznos lehet ahhoz, hogy tudja, hogy a beszúrási 1.35 rombusz lesz nagyon közelébe $ 10 000, vagy sokkal nagyobb vagy kisebb. Ábra ezt végre, nézzük rajzolása borítékba körül a regressziós egyenes, amely tartalmazza a legtöbb a pontot. A boríték nevezzük a *konfidencia-intervallum átállítása*:, hogy meglehetősen benne, hogy árak esnek a boríték, mert az elmúlt többsége van. Azt rajzolhat, hogy két további vízszintes vonalak 1.35 beszúrási amelynél az egyenes felső és alsó részén borítékot.

![Konfidencia-intervallum átállítása](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Most azt is ismertetés a konfidencia-intervallum átállítása: nyugodtan azt mondani, hogy az ár 1.35 beszúrási rombusz körülbelül $ 10 000 -, de a lehető 8 000 dolláros lehet és magas, mint a $12,000 lehet.

## <a name="were-done-with-no-math-or-computers"></a>Azt végzett, nem matematikai vagy a számítógépen

Azt jelent, hogy milyen adatok tudósok végezze el az első kifizetett, és azt jelent meg csak a rajz szerint:

* Azt feltett egy kérdést, hogy azt is válaszoljon adatokkal
* Úgy alakítottuk ki a *lineáris regressziós* használatával *modell*
* *Szövegbevitel*végeztünk, benne a *konfidencia-intervallum átállítása*

És nem használjuk matematikai vagy számítógépek adunk.

Ha további információt a Microsoft volt, naptárját...

* a Kivágás a rombusz
* Színváltozatok (mennyire a rombusz, ha éppen fehér)
* a rombusz a belefoglaláskor száma

… majd azt további hasábok volna. Ebben az esetben a matematikai lesz hasznos. Ha több mint két oszlop, akkor nehéz rajzolása képpont méretű papírra. A matematikai lehetővé teszi a sorra vagy az adatok adott sík igazítása nagyon megfelelően legyenek rendezve.

Is helyett csak néhány rombuszokká, ha két ezer vagy 2 millió volt, majd végezhető használható sokkal gyorsabb számítógépen.

Ma azt már beszélgetett lineáris regressziós módjáról, és használja az adatok előrejelzése végeztünk.

Győződjön meg arról, hogy tekintse meg a további videók a "Adatok tudományos az kezdők" a Microsoft Azure gépi tanulási.



## <a name="next-steps"></a>Következő lépések

  * [Próbálja ki egy első adatok tudományos kísérletezés a gépi tanulási Studio](machine-learning-create-experiment.md)
  * [A Microsoft Azure beszerzése a gépi tanulási bemutatása](machine-learning-what-is-machine-learning.md)
