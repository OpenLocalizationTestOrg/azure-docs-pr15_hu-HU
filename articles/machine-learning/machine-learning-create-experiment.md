<properties
    pageTitle="Egy egyszerű kísérlet a gép tanulás Studio |}] A Microsoft Azure"
    description="A gépi tanulás tankönyv végigvezeti egy egyszerű adatok tudományos kísérlet. Az ár egy autó regressziós algoritmus segítségével azt is előre."
    keywords="kísérletezni, lineáris regresszió, gépi tanulási algoritmusok, gépi tanulás tankönyv, prediktív modellezési technikák, adatok tudományos kísérlet"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Gépi tanulás oktatóprogram: hozzon létre az adatok első tudományos kísérlet Azure gép tanulás Studio

A gépi tanulás tankönyv végigvezeti egy egyszerű adatok tudományos kísérlet. Azt a lineáris regressziós modell, amely előrejelzi esetén az ár egy autót, mint például a különböző változók alapján ellenőrizze és műszaki előírások hoz létre. Ehhez fogjuk Azure gép tanulás Studio fejlesztésére és találta egy egyszerű prediktív analytics kísérletezni.

*Prediktív analytics* egyfajta megjósolni a jövőbeni eredmények aktuális adatait használó tudományos adatokat. Prediktív analytics nagyon egyszerű például adatok tudományos kezdőknek videó 4 nézni: [-előrejelzési modell egyszerű választ](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (runtime: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Hogyan segíti a gépi tanulás Studio?

Gépi tanulás Studio egyszerűen fogd és vidd modulok energiájának a prediktív modellezési technikák segítségével kísérlet beállítása. A kísérlet futtatásához, és választ találnia, *létrehozhat egy modellt*, *a modell vonat*és *pontszám és a vizsgálati minta*gép tanulás Studio fogja használni.

Adja meg a gép tanulás Studio: [https://studio.azureml.net](https://studio.azureml.net). Ha már a gép tanulás Studio előtt, kattintson a **Bejelentkezés itt**. Ellenkező esetben kattintson a **Sign Up** , és a szabad és fizetett beállítások közül választhat.

További általános információt a gép tanulás Studio lásd [Mi az a gép tanulás Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Öt kísérlet létrehozásának lépései

Oktatóanyag tanulás ezen a számítógépen, a fogja lépések öt alapvető gép tanulás Studio létrehozása, a vonat és a modell játékos kísérlet létrehozásához:

- Modell létrehozása
    - [1. lépés: Lekérdezés]
    - [2. lépés: Adatok előfeldolgozása]
    - [3. lépés: Szolgáltatások meghatározása]
- A modell vonat
    - [4. lépés: Válassza ki, és a tanuló algoritmus alkalmazása]
- Pontszám és a modell tesztelése
    - [5. lépés: Új személygépkocsi árak előrejelzésére]

[1. lépés: Lekérdezés]: #step-1-get-data
[2. lépés: Adatok előfeldolgozása]: #step-2-preprocess-data
[3. lépés: Szolgáltatások meghatározása]: #step-3-define-features
[4. lépés: Válassza ki, és a tanuló algoritmus alkalmazása]: #step-4-choose-and-apply-a-learning-algorithm
[5. lépés: Új személygépkocsi árak előrejelzésére]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>1. lépés: Lekérdezés

Számos minta adatkészletek részét képező gép tanulás Studio, amelyek közül lehet választani, és importálhatja az adatokat több forrásból. Ebben a példában a megteszünk a mintaként mellékelt dataset, **autót áras adatok (Raw)**.
Ehhez a DataSet adatkészlethez egyes gépjárművek, beleértve a márka, modell, műszaki előírások és ár számú bejegyzéseket tartalmazza.

1. Új kísérlet indításához kattintson az **+ Új** a gép tanulás Studio ablak alján, jelölje be a **kísérlet**, és válassza a **Kísérlet üres**. Válassza ki a vászon felső alapértelmezett kísérlet nevét, és nevezze következhet, például **autót ár előrejelzése**.

2. A kísérlet balra vászon az adatkészletek és a modulok a palettát. Típusú **autót** a keresőmezőbe tetején található ez a paletta a DataSet adatkészletben található feliratú **autót áras adatok (Raw)**.

    ![Paletta-keresés][screen1a]

3. A DataSet adatkészlet húzza a kísérletben vászon.

    ![Adathalmaz][screen1]

Ezek az adatok néz megtekintéséhez kattintson a kimeneti portra a személygépkocsi dataset alján, és válassza a **Megjelenítés**.

![Modul-kimeneti port][screen1c]

A változók az adathalmazban oszlopokként jelennek meg, és minden egyes példánya egy autót egy sor jelenik meg. A jobb szélen oszlopban (oszlop 26 és elnevezett "ár") a cél változó megyünk előre meg.

![A DataSet képi megjelenítés][screen1b]

A képi megjelenítések ablak bezárásához kattintson az "**x**" a jobb felső sarokban.

## <a name="step-2-preprocess-data"></a>2. lépés: Adatok előfeldolgozása

A DataSet adatkészlet gyakran van szükség bizonyos előfeldolgozása előtt kell elemezni. Előfordulhat, hogy a jelen lévő különböző sorok oszlopok hiányzó értékek megfigyelte. Ezeket a hiányzó értékeket kell tisztítani, így a modell helyesen elemezhetik az adatokat. Ebben az esetben is eltávolítása azon sorokat, amelyek értékei hiányoznak. A **Normalizált veszteségek** oszlop van így is kizárunk minden oszlopban a modellből teljesen a hiányzó értékek nagy részét.

> [AZURE.TIP] A hiányzó értékek a bemeneti adatok tisztítása előfeltétele a legtöbb modulok használatával.

Először is a **Normalizált veszteségek** oszlop eltávolítása, és majd később eltávolítása minden olyan sor, amely az adatok hiányoznak.

1. Felső részén a modul palettán az [Oszlopok kiválasztása a Dataset] kereséséhez a Keresés mezőbe írja be az **oszlopok** [ select-columns] modult, majd húzza azt a kísérlet vászon, és csatlakoztassa a kimeneti port az **autót áras adatok (Raw)** adathalmaz. Ez a modul lehetővé teszi számunkra, hogy mely oszlopokat szeretnénk figyelembe vehetjük vagy kihagyhatjuk a modell kiválasztása.

2. Jelöljük ki az [Oszlopok kiválasztása a Dataset] [ select-columns] modult a **Tulajdonságok** ablaktáblán kattintson az **oszlopkijelölő indítása** .

    - A bal oldalon kattintson **a szabályok**
    - **Kezdődik a**kattintson **az összes oszlop**. Ez arra utasítja az [Oszlopok kiválasztása a Dataset] [ select-columns] át (kivéve a kizárni kívánt vagyunk) az összes oszlopot.
    - A legördülő menük jelölje be a **kizárni** és **az oszlopnevek**, és kattintson a szövegdoboz belsejébe. Oszlopok listája jelenik meg. SELECT **Normalizált veszteségek**, és megjelenik a beviteli mező.
    - A négyzet be van jelölve (OK) gombra kattintva zárja be az oszlopkijelölő.

    ![Oszlopok kijelölése][screen3]

    A Tulajdonságok ablaktáblán az **Oszlopok kiválasztása a Dataset** azt jelzi, hogy azt keresztülmenjen minden oszlop kivéve **Normalizált veszteségek**a dataset adatkészletből.

    ![Válassza ki az oszlopok az adatkészlet tulajdonságai][screen4]

    > [AZURE.TIP] Modulra megjegyzést fűzhet, kattintson duplán a modul és a szöveg bevitele. Ennek segítségével egy szempillantással a modul tevékenységétől a kísérletben. Ebben az esetben kattintson duplán az [Oszlopok kiválasztása a Dataset] [ select-columns] modult, és írja be a Megjegyzés "kizárása normalizált veszteségek."

3. Húzza a [Hiányzó adatok tiszta] [ clean-missing-data] a kísérleti modul vászon, és csatlakoztassa az [Oszlopok kiválasztása a Dataset] [ select-columns] modul. A **Tulajdonságok** ablaktáblán jelölje ki a **teljes sor eltávolítása** eltávolítja a sorok, amelyek értékei hiányoznak az adatok tisztítása **tisztítás mód** . Kattintson duplán a modult, és írja be a megjegyzést "A hiányzó érték sorok eltávolítása."

    ![Tiszta hiányzó adatok tulajdonságai][screen4a]

4. A kísérlet alatt a kísérletben vászon **futtatása** gombra kattintva futtatható.

A kísérlet befejezését követően minden modul rendelkezik egy zöld pipa jelzi, hogy sikeresen befejeződött. A jobb felső sarokban **futó befejezett** állapotát is megfigyelhető.

![Első kísérlet futtatása][screen5]

Azt mi is tettük, a ponthoz a kísérletben mindössze tisztítsa meg az adatokat. Ha meg szeretné tekinteni a megtisztított dataset, kattintson a bal oldali kimeneti port a [Hiányzó adatok tiszta] [ clean-missing-data] modul ("Cleaned dataset"), és válassza a **Megjelenítés**. Figyelje meg a **Normalizált veszteségek** oszlop már nem része, és nincsenek megadva értékek.

Most, hogy az adatok tiszta, minden készen adja meg, milyen funkciók fogunk használni az előrejelzési modell.

## <a name="step-3-define-features"></a>3. lépés: Szolgáltatások meghatározása

A gépi tanulás *szolgáltatása* el valami érdekli az egyes mérhető tulajdonságai. A DataSet minden sor képviseli egy autót, és az egyes oszlopok jellemzője, hogy az autót.

Megtalálása egy jó funkciót egy előrejelzési modell létrehozásához szükséges kísérleti és szeretné megoldani a problémát kapcsolatos ismereteket. Egyes szolgáltatások jobbak a cél, mint a többi előrejelzéséhez. Is, néhány szolgáltatása, hogy erős korrelációt más szolgáltatásokkal (például város-mpg highway-mpg függvényében), így azok sok új információt nem hozzáadja a modellhez, és távolíthatók el.

A funkciók egy részét a DataSet használó modell most build. Térjen vissza és különböző szolgáltatásokat, futtassa újra a kísérlet, és talál, ha jobb eredményeket kaphatunk. De elindításához címűre az alábbi szolgáltatásokat:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Húzza egy másik [Oszlopok kiválasztása a Dataset] [ select-columns] a kísérlet modul vászon, és csatlakoztassa a bal oldali kimeneti port a [Hiányzó adatok tiszta] [ clean-missing-data] modul. Kattintson duplán a modult, és írja be a "Select szolgáltatások előrejelző."

2. A **Tulajdonságok** ablaktáblán kattintson az **oszlopkijelölő indítás** gombra.

3. Kattintson **a szabályok**.

4. Alatt **Kezdődik,** kattintson az **oszlopok**, és majd válassza a szűrősor **Felvétel** és **az oszlopnevek** . Adja meg az oszlop nevek listája. Ez irányítja át csak olyan oszlopot, amely azt adja meg a modult.

    > [AZURE.TIP] A kísérlet futtatásával azt is biztosítani, hogy az adatok az oszlopdefiníciók áthaladhat a dataset adatkészletből a [Hiányzó adatok tiszta] [ clean-missing-data] modul. Ez azt jelenti, hogy más modulok csatlakoztatása is az adatkészlet adatait.

5. A négyzet be van jelölve (OK) gombra.

![Oszlopok kijelölése][screen6]

A tanuló algoritmus a következő lépésekben használt adathalmazban jelenít meg. Később vissza, és próbálkozzon újra a kijelölést szolgáltatások.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>4. lépés: Válassza ki, és a tanuló algoritmus alkalmazása

Most, hogy készen áll az adatok, képzés és a tesztelés hozhat létre egy előrejelzési modell áll. Saját adatok képzése a modellt, és ellenőrizze, hogy milyen közel képes előre jelezni az árakat a modell fogjuk. Most ne aggódjon miért van szükségünk a vonat, és tesztelje a modell.

*Besorolás* és a *regressziós* kétfajta felügyelt számítógép tanulási technikák. Osztályozás előrejelzi esetén a meghatározott kategóriák, például a szín (piros, zöld vagy kék) választ. Regressziós számos előrejelzésére szolgál.

Mert az ár, amely számos előre akarunk egy regressziós modell fogjuk. Ebben a példában azt fogja vonat egy egyszerű *lineáris regressziós* modell, és a következő lépésben is teszteljük, azt.

1. Az adatokat vesszük képzés és a tesztelés külön képzés és vizsgálati készletek felosztásával. Jelölje ki és húzza a [Felosztott adatokat] [ split] a kísérleti modul vászon, és csatlakoztassa a kimenet az utolsó [Oszlopok kiválasztása a Dataset] [ select-columns] modul. **Az első adathalmaz kimeneti sorok frakció** 0,75 beállítása. Ily módon azt fogja 75 százaléka az adatok segítségével a vonat a modell, és tartsuk vissza tesztelési 25 százalék.

    > [AZURE.TIP] A **véletlen vetőmag** paraméter módosításával képzés és a tesztelés különböző véletlenszerűen mintákat hozhat létre. Ez a paraméter szabályozza a látszólagos véletlenszám-generátor történõ beoltását.

2. Futtassa a kísérlet. Ez lehetővé teszi az [Oszlopok kiválasztása a Dataset] [ select-columns] és a [Felosztott adatokat] [ split] modulok átadni az oszlopdefiníciók azt fogja ad hozzá a következő modulokat.  

3. A tanuló algoritmus, bontsa ki a **Számítógép tanulás** kategóriát, a modul paletta bal oldalán a vászon, és ezután bontsa ki a **Modell inicializálása**. Gépi tanulási algoritmusok inicializálásához használt modulok számos kategóriája jeleníti meg.

    A kísérletben, válassza a [Lineáris regressziós] [ linear-regression] (akkor is megtalálja a modul a paletta Keresés mezőbe írja be a "regressziós") **regressziós** kategóriában modul, és húzza a kísérletben vászon.

4. Keresse meg, és húzza a [Vonat modell] [ train-model] a kísérletben vászon modult. A bal oldali bemeneti port kapcsolódni a [Lineáris regressziós] kibocsátásának[ linear-regression] modul. Csatlakoztassa a megfelelő bemeneti port képzés kimeneti adatok (bal oldali port) a [Felosztott adatokat] [ split] modul.

5. Jelölje ki a [Vonat modell] [ train-model] modul, a **Tulajdonságok** ablaktáblán kattintson az **oszlopkijelölő indítás** gombra, és válassza a a **price** oszlop. Ez az érték a modell Ugrás előre.

    !["Ár" oszlop kijelölése][screen7]

6. Futtassa a kísérlet.

A pontszám, hogy új minták használt képzett regressziós modell eredménye.

![A gép tanuló algoritmus alkalmazása][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>5. lépés: Új személygépkocsi árak előrejelzésére

Most, hogy azt már képezni az adatokat a 75 százaléka a modell, azt segítségével a többi 25 %-át az adatokat, hogy milyen jól pontszám a modell funkciók.

1. Keresse meg, és húzza a [Pontszám modell] [ score-model] a kísérleti modul vászon és a bal oldali bemeneti port kapcsolódni a kimenet a [Vonat modell] [ train-model] modul. A vizsgálati adatok kimenetre (jobb oldali port) a [Felosztott adatokat] a megfelelő bemeneti port csatlakozás[ split] modul.  

    ![Pontszám modell modul][screen8a]

2. A kísérlet futtatásához, és megtekintheti a kimenet a [Pontszám modell] alapján[ score-model] modul, a kimeneti port gombra, és válassza a **Megjelenítés**. A kimeneti ár előrejelzett értékeinek és a vizsgálati adatok alapján az ismert értékeket mutatja.  

3. Végül, az eredmények minőségének teszteléséhez jelölje ki és húzza a [Modell értékelése] [ evaluate-model] a kísérleti modul vászon, és a bal oldali bemeneti port kapcsolódni a kimenet a [Pontszám modell] [ score-model] modul. (Nincsenek két bemeneti portok, mert a [Modell értékelése] [ evaluate-model] modul segítségével két modellek összehasonlítása.)

4. Futtassa a kísérlet.

A [Modell értékelése] alapján a kimenet megtekintéséhez[ evaluate-model] modul, a kimeneti port gombra, és válassza a **Megjelenítés**. A modell a következő statisztikai adatok jelennek meg:

- **Abszolút hiba jelenti.** (Van LEFOGLALVA): az átlagos abszolút hiba ( *hiba* esetén a becsült és a tényleges értéke közötti különbség).
- **Legfelső szintű átlagos sarokban hiba** (RMSE): négyzetben végzett vizsgálat adathalmazban előrejelzéseket hibák átlaga négyzetgyökét.
- **Abszolút relatív hiba**: az átlagos abszolút hibák összes tényleges értékek átlaga és a tényleges értékek közötti abszolút különbség képest.
- **Sarokban relatív hiba**: képest minden tényleges értékek átlaga és a tényleges értékek eltéréseinek sarokban hibák átlaga.
- **Együttható a meghatározása**: más néven az **R-négyzet érték**, ez az a statisztikai metrika jelzi, mennyire modell illeszkedik az adatokra.

A hiba minden egyes statisztika kisebb célszerűbb. A kisebb érték azt jelzi, hogy az előrejelzéseket jobban megfeleljen a tényleges értékek. **Együttható a meghatározása**, annál közelebb értéke egy (1.0), annál jobban előrejelzéseket.

![Értékelési eredmények][screen9]

A végső kísérletet kell néznek ki:

![Gépi tanulás oktatóprogram: prediktív modellezési technikák használó lineáris regressziós kísérlet végrehajtásához.][screen10]

## <a name="next-steps"></a>A következő lépések

Egy gép első learning oktatóprogram elvégzése után, és a kísérlet beállítása van, meg is találta a modell javítása érdekében próbálja. Módosíthatja például a szolgáltatások használata az előrejelzési. A [Lineáris regressziós] tulajdonságait módosíthatja vagy[ linear-regression] algoritmust, vagy próbálja meg egy másik algoritmust teljesen. Még több gép egyszerre tanulási algoritmusok a kísérletben való hozzáadása és hasonlítsa össze a [Modell értékelése] a két[ evaluate-model] modul.

> [AZURE.TIP] A kísérlet minden iteráció másolásához használt a kísérletben vászon alatt a **MENTÉS Másként** gombra. A kísérlet minden iteráció a vászon alatt **FUTTATNI ELŐZMÉNYEINEK megtekintése** parancsra kattintva tekintheti meg. Lásd [kezelése kísérletezni Azure gép tanulás Studio közelítés] [ runhistory] további részletekért.

[runhistory]: machine-learning-manage-experiment-iterations.md

Ha elégedett a modell, a személygépkocsi árak segítségével új adatok előrejelzésére használható webes szolgáltatásként telepítheti. Lásd: [Deploy az Azure gép tanulás webszolgáltatás] [ publish] további részletekért.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Prediktív modellezési technikák létrehozásához több kiterjedt és részletes forgatókönyv oktatás, pontozás és egy modell telepítése témakörben [Azure gép tanulás segítségével prediktív megoldás fejlesztése][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
