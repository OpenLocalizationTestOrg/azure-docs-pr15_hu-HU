<properties
   pageTitle="Kérdés feltevése fogadásához - adatokkal kérdés megfogalmazásához |} Microsoft Azure"
   description="Útmutató: adatok tudományos videó 3 kezdőknek adatok tudományos problémáját megfogalmazásához. Osztályozás és a regressziós kérdések összehasonlítása tartalmazza."
   keywords="adatok tudományos kérdések, kérdések, a regressziós kérdések, az osztályozási kérdések megfogalmazásához, éles kérdés"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Kérdés feltevése az adatok fogadásához

## <a name="video-3-data-science-for-beginners-series"></a>Videó: 3: Adatok tudományos kezdők adatsor

Útmutató: adatok tudományos videó 3 kezdőknek adatok tudományos problémáját megfogalmazásához. Ez a videó kérdések az osztályozás és a regressziós algoritmusok összehasonlítása tartalmazza.

A hatékony kihasználása a sorozat, nézze meg azokat az összes. [Nyissa meg a videók listája](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>További videók a sorozat

*Adatok tudományos kezdőknek* az öt a rövid videókból adatok tudományos rövid útmutatást talál.

  * 1 a videó: [Adatok tudományos 5 kérdésekre választ](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*
  * Videó: 2: [adatok tudományos készen áll az adatok?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 perc 56 másodperc)*
  * Videó: 3: Kérdés feltevése az adatok fogadásához
  * Videó 4: [Egyszerű modell választ előrejelzésére](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 perc 42 másodperc)*
  * 5 a videó: [Mások munka végezze el az adatok tudományos másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Átirat: Kérdés feltevése az adatok fogadásához

Üdvözli a harmadik videót a sorozat "Adatok tudományos kezdőknek."  

Ezt az egy előfordulást, a vissza néhány tipp az adatok fogadásához kérdés kidolgozásában.

Ha Ön először videókból a két korábbi a sorozat jelenhet további kijelentkezés ebből a videóból: "a 5 kérdések adatok tudományos válaszolhatnak" és "Az adatok tudományos készen áll az adatok?"

## <a name="ask-a-sharp-question"></a>Éles kérdés feltevése

Korábban azt beszélgetett hogyan adatok tudományos során a rendszer választ a kérdésére előrejelzésére (más néven kategóriák vagy feliratokat) nevek és telefonszámok használatával. De nem lehet egyszerűen bármilyen kérdés; kell egy *éles kérdést.*

Bizonytalan kérdés nem található meg egy nevet vagy egy számot kell megválaszolni. Éles kérdést kell.

Tegyük fel, egy színű lámpa egy genie, akik igazságnak megfelelően válaszolni fog bármely kérdést, akkor kérje meg a talált. De egy mischievous genie, és azt látja, hogy az adott válasz bizonytalan és áttekinthetőbb legyen, ő is igénybe vehet nem vagyok a gépnél. Vele légmentes úgy, hogy ő nem súgó, de megállapítani, hogy meg szeretné tudni egy kérdést a rögzíteni kívánt.

Ha a kérdés feltevése bizonytalan, például "Okok történjen, az árfolyam?", a genie előfordulhat, hogy választ, "az ár módosítja" volt. Ez egy truthful választ, de még nem nagyon hasznos.

De ha ez a kérdés feltevése éles, például "Mi a készlet eladási ár lesz a jövő hét?", a genie nem súgó, de adjon meg egy adott választ, és egy eladási ár előrejelzésére.

## <a name="examples-of-your-answer-target-data"></a>Példák a választ a kérdésére: célalkalmazás adatok

Amikor Ön a kérdés megfogalmazásához, ellenőrizze, hogy rendelkezik-e a válasz példák az adatok.

Ha a kérdés "Mi a készlet eladási ár lesz a jövő hét?" Ezután azt kell győződjön meg arról, hogy adatait tartalmazza, az árfolyam előzményeket.

Ha a kérdést a "saját flottában milyen autós fog először meghiúsító?" Ezután azt kell győződjön meg arról, hogy adatait tartalmazza az előző hibákkal kapcsolatos információkat.

![Célalkalmazás adatok - választ a kérdésére példákat. Adatok tudományos kérdés megfogalmazásához.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

Ezek a példák válaszok cél nevezik. A cél, mit azt próbál előrejelzésére jövőbeli adatpont, a kategória vagy szám kerül.

Ha nincs telepítve a cél adatokról, kell első néhány. Nem tudja nélküle a kérdés.

## <a name="reformulate-your-question"></a>A kérdés újraszövegezése

Megjegyzések is átfogalmazása előfordul, hogy egy még hasznosabbá választ beszerzése a kérdést.

A "Az e adatok pont vagy B?" kérdés = esetben előrejelzése kategória (vagy a név vagy címke) valamit. Válaszolja meg, hogy egy *besorolás algoritmus*használjuk.

A kérdés "Mennyi?" vagy "Hány?" = esetben előrejelzése összeget. Választ, hogy a *regressziós algoritmus*használjuk.

Ha szeretné látni, hogyan azt alakíthatják át ezeket, tekintse meg a kérdést, "melyik hírek szövegegység a olvasót legérdekesebbnek tart?" Kéri a sok lehetőségek – egyetlen választási előrejelzése más szóval "Az Ez A és B vagy C vagy D?" - és egy besorolás algoritmus célszerű használni.

De lehet, hogy ez a kérdés könnyebben fogadni, ha megjegyzések átfogalmazása azonosítóként "hogyan érdekes az egyes szövegegység ezen a listán, ez az olvasót?" Minden egyes cikk adhat most egy numerikus érték, és ezután is egyszerűen azonosíthatja a legmagasabb pontozási cikk. Most, hogy a regressziós kérdést az osztályozási kérdés fogalmazza vagy mennyire?

![A kérdés újraszövegezése. Osztályozási kérdés összehasonlítása regressziós kérdést.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Hogyan kérdése egy clue mely módszerével megkéri is ad választ.

Bizonyos családok algoritmusok – például a lehetőségekből hírek szövegegység példában szereplőhöz - egymáshoz szorosan kapcsolódó találhatók. A kérdést, amellyel a leghasznosabb válasz algoritmus is újraszövegezése.

De, legfontosabb kérje meg, hogy éles kérdés - a kérdést, az adatok fogadásához. És ellenőrizze, hogy rendelkezik-e választ, akkor a megfelelő adatokat.

Azt már beszélgetett néhány alapelvei egy kérdezzen az adatok fogadásához.

Győződjön meg arról, hogy tekintse meg a további videók a "Adatok tudományos az kezdők" a Microsoft Azure gépi tanulási.


## <a name="next-steps"></a>Következő lépések

  * [Próbálja ki egy első adatok tudományos kísérletezés a gépi tanulási Studio](machine-learning-create-experiment.md)
  * [A Microsoft Azure beszerzése a gépi tanulási bemutatása](machine-learning-what-is-machine-learning.md)
