<properties
    pageTitle="Gépi tanulási modell eredmények értelmezése |} Microsoft Azure"
    description="Hogyan lehet választani a optimális beállítása megjelenítésére pontszámhoz modell kimeneti értékeket és használatával algoritmus."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Azure gépi tanulási modell eredmények értelmezése

Ebből a témakörből megtudhatja, hogyan és ábrázolása az Azure gépi tanulási Studio előrejelzési eredmények értelmezéséhez. A modell képzett, és a munkavégzés az előrejelzések fölött ("pontszáma a modell"), szükség, és az előrejelzési eredmény értelmezése.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Vannak gépi tanulási az Azure gépi tanulási levő négy fő típusú:

* Besorolás
* Fürtözés
* Regressziós
* Recommender rendszerek

Ezek a modellek fölött szövegbevitel használt modulokat a következők:

* [Modell pontszám] [ score-model] osztályozás és a regressziós modul
* [Fürt hozzárendelése] [ assign-to-clusters] fürtözéshez modul
* [Matchbox Recommender pontszám] [ score-matchbox-recommender] ajánlási rendszerhez

A dokumentum adatainak értelmezéséhez előrejelzési eredményt az egyes modulok ismerteti. Ezeket a modulokat áttekintése megtudhatja, [hogy miként optimalizálhatja az Azure gépi tanulási a algoritmusok paraméterek válassza](machine-learning-algorithm-parameters-optimize.md).

Ez a témakör a szövegbevitel értelmezése, de nem modell kiértékelés címeket. További információt a modell felmérése, akkor olvassa el a [hogyan Azure gépi tanulási modell teljesítménye ki szeretné számítani](machine-learning-evaluate-model-performance.md).

Ha még kezdő az Azure gépi tanulási és első lépések egy egyszerű kísérlet segítséget nyújt, olvassa el a [egy egyszerű kísérlet Azure gépi tanulási Studio létrehozása](machine-learning-create-experiment.md) az Azure gépi tanulási Studio című témakört.

## <a name="classification"></a>Besorolás ##
Van két alkategóriák besorolás problémákat:

* Csak két osztályok (két szintű vagy bináris osztályozás) kapcsolatos problémák
* Kettőnél több osztályok (több osztály osztályozás) kapcsolatos problémák

Azure gépi tanulási vannak a különböző modulok kell foglalkozni az egyes besorolás ilyen típusú, de az előrejelzési eredmények értelmezéséhez módszerei hasonlítanak.

### <a name="two-class-classification"></a>Második szintű besorolás###
**Példa kísérlet**

Példa két szintű besorolás probléma a iris virágok besorolása. Azok a funkciók alapján iris virágok osztályozásához, akkor a feladatot. Az Iris adatkészlet adni az Azure gépi tanulási van a népszerű [Iris adatkészlet](http://en.wikipedia.org/wiki/Iris_flower_data_set) egy részét tartalmazó példányok szóló csak két virágos (0 és 1 osztályok). Vannak olyan négy funkcióinak minden virágos (sepal hossza, sepal szélességét, szirom hossza és szirom szélesség).

![Képernyőkép a iris kísérlet](./media/machine-learning-interpret-model-results/1.png)

Ábra 1. IRIS két szintű besorolás probléma kísérlet

Egy kísérlet történt a probléma megoldásához 1 ábrán látható módon. Második szintű csillapítja döntési fa modell képzett, és a pontszáma. Most már meg is jelenítse meg az előrejelzési eredményeket a [Pontszámhoz modell] [ score-model] a kimeneti port [Pontszámhoz modell] kattintva modul[ score-model] modult, és válassza a **Megjelenítés**.

![Pontszámhoz modell modul](./media/machine-learning-interpret-model-results/1_1.png)

Ekkor megjelenik a pontozási eredménye 2 ábrán látható módon.

![Iris két szintű besorolás kísérlet eredményei](./media/machine-learning-interpret-model-results/2.png)

Ábra 2. Második szintű osztályozás pontszámhoz modell eredményt ábrázolása

**Eredmény értelmezése**

Az eredmények táblázat hat oszlop van. A bal oldali négy adatoszlopot négy funkcióját tartalmazza. A jobb oldali két oszlopot, pontszáma pedig a valószínűség pontszáma az előrejelzési eredmények. A valószínűség pontszáma oszlopban látható, hogy a valószínűség, amelyhez tartozik egy virágokkal a pozitív osztály (osztály-1). Például az első szám (0.028571) oszlop jelez is 0.028571 valószínűségét, hogy az első virágos osztály 1 tartozik. A címkék pontszáma oszlopban látható, hogy az egyes virág előre jelzett osztálya. A valószínűség pontszáma oszlop alapul. Ha scored valószínűségértékének inverzét számítja ki a virágos 0,5-nél nagyobb, osztály 1 előre jelzett. Egyéb esetben azt az előre jelzett osztály 0.

**Webes kiadvány-szolgáltatás**

Miután az előrejelzési eredmények értelmezni, és ítélt hang, a kísérlet tehetők közzé, webes szolgáltatás, hogy beállítaná őket különböző alkalmazásokat, és hívja meg a minden új iris virágos osztály előrejelzések beszerzése. Egy oktatás kísérlet átalakítása egy pontozási kísérlet és a projekt közzététele a webes szolgáltatásként című témakörben talál [az Azure gépi tanulási webszolgáltatás közzététel](machine-learning-walkthrough-5-publish-web-service.md). Ez az eljárás biztosít a pontozási kísérlet 3 ábrán látható módon.

![Képernyőkép a kísérlet pontozás](./media/machine-learning-interpret-model-results/3.png)

Ábra 3. A iris két szintű besorolás probléma kísérlet pontozás

Most már kell beállítania a bemeneti és kimeneti esetében a webszolgáltatás. A bemeneti érték a megfelelő bemeneti port [Pontszámhoz]modell[score-model], amely olyan, a Iris virágos szolgáltatásai beviteli. A választási lehetőségek, a kibocsátás attól függ, hogy érdeklik a becsült osztály (pontszáma címke) vagy scored annak valószínűségét számítja ki. Ebben a példában feltételezzük, hogy érdeklik is. Jelölje ki a kívánt kimeneti oszlopokat, használja a [Jelölje ki az adatkészlet oszlopait] [ select-columns] modulra. Kattintson a [Jelölje ki az adatkészlet oszlopait][select-columns] **oszlopkijelölő indítása**kattintson, és válassza ki a **Címkék pontszáma** és **Pontszáma valószínűség**. [Jelölje ki az adatkészlet oszlopait] , a kimeneti port beállítása után[ select-columns] és újra fut, akkor készen áll teheti közzé a pontozási kísérlet webszolgáltatás **WEBSZOLGÁLTATÁS közzététele**gombra kattintva. A végleges kísérlet ábra 4 néz ki.

![A iris két szintű besorolás kísérlet](./media/machine-learning-interpret-model-results/4.png)

Ábra 4. Végleges pontozási kísérlet iris két szintű besorolás frissíthetőségével kapcsolatos problémák

Miután futtassa a webszolgáltatás, és adja meg a próba-példány néhány funkció érték, az eredmény a két szám adja eredményül. Az első szám scored felirat, a második pedig scored annak valószínűségét számítja ki. Ez a virágos osztály 1 0.9655 valószínűségével előre jelzett a van.

![Értelmezése pontszámhoz modell tesztelése](./media/machine-learning-interpret-model-results/4_1.png)

![A pontozási vizsgálati eredmények](./media/machine-learning-interpret-model-results/5.png)

Ábra 5. Webes szolgáltatás eredménye iris két szintű besorolás

### <a name="multi-class-classification"></a>Több elem osztály besorolás
**Példa kísérlet**

Ez a kísérlet az multiclass osztályozás példaként betű-felismerés tevékenység végrehajtása. A besorolás megkísérli előrejelzésére adott betűvel (osztály) néhány a kézzel írt képek kiolvasott kézzel írott attribútum értékek alapján.

![Példa a levél felismerés](./media/machine-learning-interpret-model-results/5_1.png)

Az oktatóanyag adatok nincsenek kézzel írott betű képek kiolvasott 16 funkciók. 26 betűje a 26 osztályok alkotnak. Ábra 6 beállítása próba adatkészlet funkciót, amely egy betű felismerés multiclass besorolás modellt képzése és előrejelzésére kísérlet látható.

![Levél felismerés multiclass besorolás kísérlet](./media/machine-learning-interpret-model-results/6.png)

6 ábrán. Levél felismerés multiclass besorolás probléma kísérlet

Az eredmények a [Pontszámhoz modell] megjelenítése[ score-model] a kimeneti port [Pontszámhoz] modell kattintva modul[ score-model] modult, és válassza a **Megjelenítés**, meg kell jelennie a tartalom 7 ábrán látható módon.

![Pontszámhoz modell eredménye](./media/machine-learning-interpret-model-results/7.png)

Ábra 7. Több elem osztály osztályozás pontszámhoz modell eredmények ábrázolása

**Eredmény értelmezése**

A bal oldali 16 oszlopokat jelenítik meg a próba-készlet szolgáltatás értékeket. Nevek, például a "XX" osztály pontszáma valószínűség, csak az oszlopokat, például abban az esetben, két szintű a valószínűség pontszáma oszlopban. Mutatnak annak valószínűségét számítja ki, hogy a megfelelő tétel tartozik egy bizonyos osztály. Ha például az első bejegyzés van 0.003571 annak valószínűségét, egy "A;" 0.000451 valószínűségét, hogy az "B", és így tovább. Az utolsó oszlop (pontszáma feliratok) megegyezik a címkék pontszáma két szintű kezdőbetűkkel. Az osztály a legnagyobb scored valószínűségével kijelöli a megfelelő tételének előre jelzett osztály szerint. Például az első bejegyzés scored felirat is "F" mivel ez a legnagyobb valószínűség kell egy "F" (0.916995).

**Webes kiadvány-szolgáltatás**

Az egyes bejegyzések és számítja ki a scored címke scored felirat is megnyithatja. Az egyszerű logika, hogy megtalálja a legnagyobb a valószínűség, a scored valószínűség között. Ehhez használja az [R parancsfájl végrehajtása] szükséges[ execute-r-script] modulra. Ábra 8 jelenik meg a R kódot, és a kísérlet az eredmény jelenik meg a 9.

![R példa](./media/machine-learning-interpret-model-results/8.png)

Ábra 8. R kódját pontszáma címkéket és a kapcsolódó valószínűség, a címkék kibontása

![Kísérlet eredményének](./media/machine-learning-interpret-model-results/9.png)

Ábra 9. A betű-felismerés multiclass besorolás probléma végleges pontozási kísérlet

Miután közzététele és futtassa a webszolgáltatás, és adja meg az egyes beviteli szolgáltatás értékeket, a visszaadott eredmény néz ki ábra 10. Ez a kézzel írt levél, a kibontott 16 funkcióival van előre jelzett 0.9715 valószínűségével "ma" lesz.

![Értelmezése pontszámhoz modul tesztelése](./media/machine-learning-interpret-model-results/9_1.png)

![Próba eredménye](./media/machine-learning-interpret-model-results/10.png)

Ábra 10. Webes szolgáltatás eredménye multiclass besorolás

## <a name="regression"></a>Regressziós

Regressziós problémákról besorolás problémák eltérnek. Osztályozási probléma megkísérli előrejelzésére különálló osztályok, amelyek például egy iris virágos osztály tartozik. De a regressziós probléma a következő példában látható, mint kívánt előrejelzésére folyamatos változó, például egy autó árát.

**Példa kísérlet**

A példa a regressziós rakodótere ár előrejelzési használnak. A funkciói, köztük a helyezése, üzemanyag-típus, törzsébe írja be vagy meghajtó ikon alapján autót árát előrejelzésére próbálja. A kísérlet 11-es szám jelenik meg.

![Rakodótere ár regressziós kísérlet](./media/machine-learning-interpret-model-results/11.png)

Ábra 11. Rakodótere ár regressziós probléma kísérlet

Az [Eredmény modell] megjelenítése[ score-model] modul, az eredmény az alábbihoz hasonló ábra 12.

![Eredmények pontozási rakodótere ár előrejelzési probléma](./media/machine-learning-interpret-model-results/12.png)

Ábra 12. A rakodótere ár előrejelzési probléma pontozási eredménye

**Eredmény értelmezése**

A címkék pontszáma következő pontozási ennek eredménye oszlopára. A számok előre jelzett ára az egyes autós.

**Webes kiadvány-szolgáltatás**

A regressziós kísérlet be egy webszolgáltatásból közzéteheti és módon, mint például a két szintű besorolás használatieset rakodótere ár szövegbevitel hívja meg.

![Kísérlet pontozási rakodótere ár regressziós probléma](./media/machine-learning-interpret-model-results/13.png)

Ábra 13-mal. A pontozási rakodótere ár regressziós hibát kísérlet

A webszolgáltatás fut, a visszaadott eredmény az alábbihoz hasonló ábra 14. A becsült árát a autós $15,085.52.

![Tesztelje a pontozási modul értelmezése](./media/machine-learning-interpret-model-results/13_1.png)

![A pontozási modul eredménye](./media/machine-learning-interpret-model-results/14.png)

Ábra 14. Webes szolgáltatás eredménye rakodótere ár regressziós frissíthetőségével kapcsolatos problémák

## <a name="clustering"></a>Fürtözés

**Példa kísérlet**

Vegyük használatával Iris adatkészlet ismét egy fürtképző kísérlet. Itt szűrhet ki az osztályjegyzetfüzet címkék adatok megadása, hogy csak funkciókat és fürtözéshez is használható. A iris használatieset, adja meg a csoportok a oktatás folyamat során, ami azt jelenti, hogy a virágok a két viselkedésre volna fürt kettő számát. A kísérlet 15 ábrán látható.

![Kísérletezés Iris fürtképző probléma](./media/machine-learning-interpret-model-results/15.png)

Ábra 15. Kísérletezés Iris fürtképző probléma

Fürtözés különbözik besorolás hogy oktatás adatkészlet nincs alapokról-igazság címkék önmagában. A csoportok a distinct fürt oktatás adatkészlet példányokat fürtképző. A tanfolyamok során a modell elemeit címkék által tanulási a saját szolgáltatások közötti különbségeket. Ezt követően a képzett modell használható további sorolják be a következő elemekre. A keresési eredményt azt a fürtképző probléma belül a két részből áll. Az első rész oktatás adatkészlet van parancsot, majd a második van osztályozásához és a képzett modellt új adatkészletet.

Az eredmény első része a bal oldali kimeneti port [vonaton fürtözés] modell kattintva megjeleníthetők[ train-clustering-model] majd a **Megjelenítés**parancsra. A Megjelenítés 16 ábrán látható.

![Fürtképző eredménye](./media/machine-learning-interpret-model-results/16.png)

Ábra 16. Adatkészlet számára az oktatás eredmény fürtözés ábrázolása

A második részben, az új bejegyzések a képzett fürtképző modellel fürtözés eredményét ábra 17 jelenik meg.

![Eredmény fürtözés ábrázolása](./media/machine-learning-interpret-model-results/17.png)

Ábra 17. Új adatkészletet eredmény fürtképzés ábrázolása

**Eredmény értelmezése**

Bár a két részből eredményét kísérletet különböző szakaszok erednek, feltétlenül lesz azonos, és az adott dokumentumkészletből ugyanúgy értelmezi. Az első négy oszlop funkcióját tartalmazza. Az utolsó oszlopban, a hozzárendelések, az előrejelzési eredmény. A bejegyzéseket, ugyanazt a számot rendelt vannak előrejelzett el szeretné helyezni a azonos fürt, ez azt jelenti, hogy információikat hasonlóságokat valamilyen módon (a kísérlet használja az alapértelmezett Euclidean távolság mérőszám). Mivel a megadott fürt 2-nek számát, a hozzárendelések elemeit címkézett vannak 0 vagy 1.

**Webes kiadvány-szolgáltatás**

Tegye közzé a fürtképző kísérlet be egy webszolgáltatásból, és hívja meg, ahogy a két szintű besorolás ugyanúgy használati eset előrejelzések fürtözéshez.

![Kísérlet pontozási iris fürtképző probléma](./media/machine-learning-interpret-model-results/18.png)

Ábra a 18. A pontozási iris fürtképző hibát kísérlet

A webszolgáltatás futtatása után a visszaadott eredmény az alábbihoz hasonló ábra 19. Ez a virágos van előre jelzett fürt 0 lesz.

![Teszt pontozási modul értelmezése](./media/machine-learning-interpret-model-results/18_1.png)

![A pontozási modul eredménye](./media/machine-learning-interpret-model-results/19.png)

Ábra 19. Webes szolgáltatás eredménye iris két szintű besorolás

## <a name="recommender-system"></a>Recommender rendszer
**Példa kísérlet**

Recommender rendszerek esetében is használhatja az étterem ajánlási probléma szerepel példaként: is javasoljuk éttermek, azok értékelése az előzmények alapján ügyfeleknek. A bemeneti adatok három részből áll:

* Az ügyfelek éttermi minősítése
* A szolgáltatás ügyféladatok
* Éttermi szolgáltatás adatok

Több beállítást meg azt a [Vonaton Matchbox Recommender] végezhető műveletek[ train-matchbox-recommender] Azure gépi tanulási moduljának:

- Egy adott felhasználó és a elem minősítése előrejelzésére
- Elemek javasoljuk, hogy az adott felhasználó
- Keresse meg a felhasználók az adott felhasználó kapcsolódó
- Egy adott elem keresése

Megadhatja, hogy mit szeretne tenni a **Recommender előrejelzési milyen** menüben a négy lehetőség kiválasztásával. Itt a négy minden esetben is végigvezetik.

![Matchbox recommender](./media/machine-learning-interpret-model-results/19_1.png)

Egy tipikus Azure gépi tanulási kísérlet recommender rendszer ábra 20 néz ki. E recommender rendszer modulokat használatával kapcsolatos további tudnivalókért lásd: [vonaton matchbox recommender] [ train-matchbox-recommender] és [pontszámhoz matchbox recommender][score-matchbox-recommender].

![Recommender rendszer kísérlet](./media/machine-learning-interpret-model-results/20.png)

Ábra 20. Recommender rendszer kísérlet

**Eredmény értelmezése**

**Egy adott felhasználó és a elem minősítése előrejelzésére**

Jelölje ki a **Minősítési előrejelzési** **Recommender előrejelzési típusa**csoportban, a recommender rendszer egy adott felhasználó és a elem minősítése előrejelzésére olya. A megjelenítés, a [Pontszámhoz Matchbox Recommender] [ score-matchbox-recommender] kimeneti ábra 21 néz ki.

![A recommender rendszer – előrejelzési minősítés eredményét pontszám](./media/machine-learning-interpret-model-results/21.png)

Ábra 21. A recommender rendszer – előrejelzési minősítés pontszámhoz eredményét ábrázolása

Az első két oszlop a felhasználó-elem párokká, a bemeneti adatok által biztosított. A harmadik oszlop a felhasználó bizonyos elem előre jelzett minősítése. Például az első sor ügyfél U1048 várhatóan ráta éttermi 135026 2.

**Elemek javasoljuk, hogy az adott felhasználó**

Jelölje ki az **Elem ajánlási** **Recommender előrejelzési típusa**csoportban, a recommender rendszer elemek javasoljuk, hogy az adott felhasználó mintaüzenetet. Ebben az esetben válassza ki az utolsó paramétere *ajánlott elem kiválasztása*. A beállítás **A névleges elemeket (a modell értékelést)** nem elsősorban a modell kiértékelés a tanfolyamok során. Szövegbevitel ebben a szakaszban válassza azt **Az összes elem**. A megjelenítés, a [Pontszámhoz Matchbox Recommender] [ score-matchbox-recommender] kimeneti ábra 22 néz ki.

![Recommender rendszer – elem ajánlási pontszámhoz eredménye](./media/machine-learning-interpret-model-results/22.png)

Ábra 22. A recommender rendszer – elem ajánlási pontszámhoz eredményét ábrázolása

Az első hat oszlopok jelöli az adott felhasználói azonosítók javasoljuk az elemek, mint a bemeneti adatok által biztosított A többi öt oszlop a cikkeknek ajánlott, hogy a felhasználó relevancia csökkenő sorrendben. Az első sor, például a legtöbb ajánlott éttermi vevő U1048 134986, 135018, 134975, 135021 és 132862 követ is.

**Keresse meg a felhasználók az adott felhasználó kapcsolódó**

Jelölje ki a **Kapcsolódó felhasználók** **Recommender előrejelzési típusa**csoportban, a kapcsolódó felhasználóknak, hogy az adott felhasználó keresése recommender rendszer mintaüzenetet. Kapcsolódó felhasználók olyan felhasználók rendelkezzenek hasonló beállítások. Ebben az esetben válassza ki az utolsó paramétere *kapcsolódó felhasználó kiválasztása*. A beállítás **A felhasználóknak, hogy minősítette elemek (a modell értékelést)** nem elsősorban a modell kiértékelés a tanfolyamok során. Válassza **Az összes felhasználó** előrejelzési ebben a szakaszban. A megjelenítés, a [Pontszámhoz Matchbox Recommender] [ score-matchbox-recommender] kimeneti néz 23 ábrán.

![Recommender rendszer – kapcsolódó felhasználók pontszámhoz eredménye](./media/machine-learning-interpret-model-results/23.png)

Ábra 23. Pontszámhoz eredményeit a recommender rendszer – a kapcsolódó felhasználók ábrázolása

Az első hat oszlopok jeleníti meg az adott felhasználó lehetővé tevő dokumentumazonosítók használatához szükséges kapcsolódó felhasználója, keresse a bemeneti adatok által biztosított. A többi öt oszlop szerint csökkenő sorrendben jelentősége annak a felhasználónak a becsült kapcsolódó felhasználók tárolja. Az első sor, például a leginkább megfelelő vevő vevő U1048 U1051, U1066, U1044, U1017 és U1072 követ is.

**Egy adott elem keresése**

Jelölje ki a **Kapcsolódó elemek** **Recommender előrejelzési típusa**csoportban, egy adott elemhez kapcsolódó elemek keresése a recommender rendszer olya. Kapcsolódó elemek a nagy valószínűséggel kell tetszett az adott felhasználó által elemek. Ebben az esetben válassza ki az utolsó paramétere *kapcsolódó elem kiválasztása*. A beállítás **A névleges elemeket (a modell értékelést)** nem elsősorban a modell kiértékelés a tanfolyamok során. Ebben a szakaszban előrejelzési azt válassza **Az összes elem** . A megjelenítés, a [Pontszámhoz Matchbox Recommender] [ score-matchbox-recommender] kimeneti ábra 24 néz ki.

![Recommender rendszer – a kapcsolódó elemek pontszámhoz eredménye](./media/machine-learning-interpret-model-results/24.png)

Ábra 24. Jelenítse meg a recommender rendszer – a kapcsolódó elemek pontszámhoz eredményeket

Az első hat oszlopot az adott elem megkereséséhez szükséges elemekhez, mint a bemeneti adatok által biztosított azonosítók jelöli. A többi öt oszlop csökkenő sorrendben értelmez jelentősége annak az elemnek a becsült kapcsolódó elemek tárolja. Például az első sor és a legfontosabb elem 135026 elem 135074, 135035, 132875, 135055 és 134992 követ.

**Webes kiadvány-szolgáltatás**

A folyamat – ezek kísérletek közzétételi webszolgáltatásokhoz előrejelzések megszerezni, hasonlít az egyes az négy jelenik meg. A második példa (az adott felhasználó elemek ajánlott) az alábbi példában vesszük. Kövesse az egyéb három ugyanezt az eljárást.

A képzett recommender rendszer mentése képzett modellben, és a bemeneti adatok egyetlen felhasználói azonosító oszlop szűrése a kérésének, ábra 25 ahogy a kísérlet jelölje be a csatlakoztatni, és tegye közzé egy webszolgáltatásból.

![A éttermi ajánlási probléma kísérlet pontozás](./media/machine-learning-interpret-model-results/25.png)

Ábra 25. A éttermi ajánlási probléma kísérlet pontozás

A webszolgáltatás fut, a visszaadott eredmény az alábbihoz hasonló ábra 26. A felhasználó U1048 öt ajánlott éttermek 134986, 135018, 134975, 135021 és 132862.

![Minta recommender rendszer szolgáltatás](./media/machine-learning-interpret-model-results/25_1.png)

![Eredményminta kísérlet](./media/machine-learning-interpret-model-results/26.png)

Ábra 26. Éttermi ajánlási probléma Web service eredménye


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
