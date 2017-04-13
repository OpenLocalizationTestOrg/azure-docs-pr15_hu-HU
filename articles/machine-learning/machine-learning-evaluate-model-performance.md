<properties 
    pageTitle="Gépi tanulási modell teljesítmény felmérése |} Microsoft Azure" 
    description="Megtudhatja, hogyan Azure gépi tanulási modell teljesítménye ki szeretné számítani." 
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
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Hogyan kell kiértékelni Azure gépi tanulási modell teljesítménye

Ez a témakör bemutatja, hogyan lehet a teljesítmény Azure gépi tanulási Studio modell ki szeretné számítani, és lehetővé teszi a mérési módja miatt rövid ismertetés a feladat végrehajtásához. Három esetei felügyelt tanulási jelennek meg: 

* regressziós
* bináris besorolás 
* multiclass besorolás

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Modell teljesítményének értékelése az egyik alapvető szakaszok a adatok tudományos folyamat. Azt jelzi, hogy hogyan sikeres egy adathalmaz pontozási (előrejelzések) Várjon egy képzett modell. 

Azure gépi tanulási támogatja a modell kiértékelés két a fő gépi tanulási modulok keresztül: [Felmérése modell] [ evaluate-model] és [Idegen érvényesítése modell][cross-validate-model]. Ezeket a modulokat látni, hogyan hajtja végre a modell értelmez számos gyakran használt gépi tanulási és a statisztikát mértékek teszi lehetővé.

##<a name="evaluation-vs-cross-validation"></a>Kiértékelés összehasonlítása határokon érvényesítése##
Kiértékelés és közötti érvényességi a teljesítmény a modell mérésére szokásos módon. Kiértékelés mértékek, nézze meg, vagy vetheti össze és az egyéb modellek létrehozása mindkettő.

[Modell felmérése] [ evaluate-model] scored adatkészletet vár beviteli (vagy abban az esetben hasonlítsa össze 2 különböző modellek teljesítményét szeretne 2). Ez azt jelenti, hogy szeretne-e a modell [Láthatja el ismeretekkel a modell] képzése[ train-model] néhány adatkészlet [Pontszámhoz modellt] használja a modul, és ellenőrizze az előrejelzések[ score-model] modul, mielőtt kiértékelheti az eredményeket. A kiértékelés értéke igaz címkék, amelyek mindegyike a [Pontszámhoz modell] kimeneti együtt scored címkék/valószínűség alapján[ score-model] modulra.

Azt is megteheti használhatja közötti érvényességi vonaton pontszámhoz felmérése műveletek (10 éleket) a webhelygazdák számos automatikusan a különböző részhalmazának a bemeneti adatok. A bemeneti adatok 10 kijelzők, ahol egy van fenntartva teszteléshez, és az egyéb 9 – oktatás oszlik. Ez a folyamat ismétlődik 10 időpontok, és a kiértékelés mérési módja miatt átlagolni vannak. Annak megállapítása, hogy mennyire szeretne generalize modell az új adatkészletek Ez segít. A [Modell határokon érvényesítése] [ cross-validate-model] modul egy kellő modell és néhány címkézett adatkészlet tart, és exportálja az kiértékelés eredményt az egyes az 10 éleket, az átlagolt eredmények mellett.

Az alábbi szakaszok azt fogja egyszerű besorolás és a regressziós modellek összeállítása és mindkét a [Kiértékelendő modell] használata a teljesítmény elérése érdekében felmérése[ evaluate-model] és a [Modell határokon érvényesítése] [ cross-validate-model] modulokat.

##<a name="evaluating-a-regression-model"></a>A regressziós modell értékelése##
Tegyük fel, szeretnénk előrejelzésére egy autó ár használata bizonyos szolgáltatások, például a méretét, lóerő, motor specs és így tovább. Ez a szokásos regressziós probléma, ahol a cél változó (*ár*) folyamatos numerikus értéket. Azt, hogy egy bizonyos autó szolgáltatás értékeket megadni, hogy autós árát is előrejelzésére egyszerű lineáris regressziós modell elfér. A regressziós modell pontszám az azonos adatkészlet azt a képzett használható. Van még a autók tartozó előrejelzett árakat, amint azt kiértékelésének eredménye a teljesítmény a modell mennyi az előrejelzések eltérnek a tényleges árak átlagosan megjeleníti. Ezt mutatják be, hogy a elérhető *autót ár (termék) adatainak adatkészlet* Azure gépi tanulási Studio **Mentett adatkészleteket** szakasza használjuk.
 
###<a name="creating-the-experiment"></a>A kísérlet létrehozása###
A következő modulokat hozzáadása az Azure gépi tanulási Studio a munkaterületet:

- Rakodótere ár adatok (nyers)
- [Lineáris regressziós][linear-regression]
- [Vonaton modell][train-model]
- [Pontszámhoz modell][score-model]
- [Modell felmérése][evaluate-model]


Csatlakozás a portokat, ahogy alább látható módon az 1 és beállítása a címke oszlop [Vonaton modell] [ train-model] *ár*modulra.
 
![A regressziós modell értékelése](media/machine-learning-evaluate-model-performance/1.png)

Ábra 1. A regressziós modell értékelése.

###<a name="inspecting-the-evaluation-results"></a>A kiértékelés eredmények vizsgálata###
Után a kísérlet fut, a kimeneti port a [Modell kiértékelése] , hogy kattint[ evaluate-model] modul, és válassza a *Megjelenítés* kiértékelés eredmények megjelenítéséhez. A regressziós modellek elérhető kiértékelési mérés: *Abszolút hibaüzenet jelent*, *Legfelső szintű abszolút hibaüzenet jelent*, *A relatív, abszolút hiba*, *Relatív négyzet hiba*és a *Együtthatóját az meghatározása*.

A "hiba" az alábbi kifejezés a becsült és a true érték közötti eltérés időmennyiségét. Abszolút értékét, vagy a különbség a négyzete vannak általában számított minden példányára, a teljes nagyságát hiba rögzítéséhez, a becsült és a true érték közötti különbség negatív bizonyos esetekben lehet. A hiba mérési módja miatt mérje le a regressziós modellben levő az IGAZ értéket az előrejelzések középérték szórása pedig a prediktív teljesítménye. Alsó hibaértékek jelenti azt a modell pontosabb ütemezhetőség érdekében, hogy az előrejelzések. Egy általános mutatója 0 azt jelenti, hogy a modell tökéletesen illeszkedik-e az adatok.

Meghatározása, amely más néven R együtthatója négyzet, mérési mennyire minta illeszkedik-e az adatokhoz szabványos módon is. A modell magyarázható változat arányában értelmezhetők. Magasabb szintű arányok szerint célszerűbb ebben az esetben, ha 1 azt jelzi, tökéletes.
 
![Lineáris regressziós kiértékelés mérőszámok](media/machine-learning-evaluate-model-performance/2.png)

Ábra 2. Lineáris regressziós kiértékelés mértékek.

###<a name="using-cross-validation"></a>Adatérvényesítési tartományok használata###
A korábban említett hajthatja végre ismételt oktatás, pontozási és használatával automatikusan a [Modell határokon érvényesítése] értékelést[ cross-validate-model] modulra. Ebben az esetben szükség egy adatkészletet, egy kellő modell és [Idegen érvényesítése modell] [ cross-validate-model] modul (lásd az alábbi ábra). Megjegyzés kell beállítása a címke oszlop *ár* a [Modell határokon érvényesítése] [ cross-validate-model] modul tulajdonságait.

![Idegen-regressziós modell érvényesítése](media/machine-learning-evaluate-model-performance/3.png)

Ábra 3. Regressziós modell határokon érvényesítése.

Után a kísérlet fut, kattintson a jobb oldali kimeneti port a [Modell határokon érvényesítése] is vizsgálata értékelési eredmények[ cross-validate-model] modulra. A részletes adatok megjelenítése a mértékek törzsének (nyíló), és a mérési módja miatt (ábra 4) minden egyes átlagolt eredményeinek fog szolgálni.
 
![A regressziós modell határokon-ellenőrzési eredményeinek](media/machine-learning-evaluate-model-performance/4.png)

Ábra 4. A regressziós modell határokon-ellenőrzés eredménye.

##<a name="evaluating-a-binary-classification-model"></a>Bináris besorolás modell értékelése##
Bináris besorolás esetben a cél változó csak két lehetséges eredmények, például van: {0; 1} vagy {hamis, igaz}, {negatív, pozitív}. Tegyük fel, akkor kapnak az egyes felnőtt alkalmazottak adatkészletet demográfiai és employment változók, és hogy rendszer kéri, hogy a bevétel szint értékekkel bináris változó előrejelzésére {"< 50K =", "> 50K"}. Ez azt jelenti a negatív osztály az alkalmazottak, akik legfeljebb 50 k évente, pedig pozitív osztály összes alkalmazott. A regressziós esetben, ahogy azt volna láthatja el ismeretekkel a modell, bizonyos adatok pontszám és az eredmények értékelése. A fő különbség az Azure gépi tanulási kiszámít mértékek és a kimeneti értékeket. A bevétel szintű előrejelzési forgatókönyv ábrázolására, fogjuk használni a [felnőtt](http://archive.ics.uci.edu/ml/datasets/Adult) adatkészlet létrehozása az Azure gépi tanulási kísérlet és a teljesítmény két szintű logisztikus regresszió modell, gyakran használt bináris besorolás felmérése.

###<a name="creating-the-experiment"></a>A kísérlet létrehozása###
A következő modulokat hozzáadása az Azure gépi tanulási Studio a munkaterületet:

- Felnőtt népszámlálás bevétel bináris besorolás adatkészlet
- [Második szintű logisztikus regresszió][two-class-logistic-regression]
- [Vonaton modell][train-model]
- [Pontszámhoz modell][score-model]
- [Modell felmérése][evaluate-model]

Csatlakozás a portokat, az 5 alább látható módon, és a [Vonaton adatmodell] felirat oszlopát állítsa[ train-model] modul *jövedelem*.

![Bináris besorolás modell értékelése](media/machine-learning-evaluate-model-performance/5.png)

Ábra 5. Bináris besorolás modell értékelése.

###<a name="inspecting-the-evaluation-results"></a>A kiértékelés eredmények vizsgálata###
Után a kísérlet fut, a kimeneti port a [Modell kiértékelése] , hogy kattint[ evaluate-model] modult, és válassza a *Megjelenítés* a kiértékelés eredmények (ábra 7). A rendelkezésre álló bináris besorolás modellek kiértékelési mérés: *pontosságát*, *pontosság*, *visszahívása*, *F1 pontszámhoz*és *AUC*. Ezenkívül a modul jelenít meg egy pozitív igaz, hamis negatív, téves, és IGAZ negatív, valamint *ROC* *Pontosság/visszahívási*és *emelje fel a* görbék számát megjelenítő fejetlenséget mátrix.

Pontosabb egyszerűen megfelelően minősített példányok arányát. Érdemes általában az első metrikus megnézi besorolás kiértékelésekor. Jó helyen jár, ha a tesztadatokat van kiegyenítetlenségét (ahol a legtöbb példányok része ilyen az osztályok), illetve további érdeklik vagy az egyik az osztályok a teljesítmény a pontosság nem igazán rögzítése besorolás hatékonyságát. Bevétel szintű besorolás esetben tegyük fel, bizonyos adatok, ahol példányok 99 %-os jelenítik meg hírnévpontszámokat legfeljebb 50 k évente, akik a tesztelése. Az osztály-beli által egy 0.99 pontosság eléréséhez lehetséges "< 50K =" összes előfordulását. A besorolás ebben az esetben valószínűleg egy jó feladat általános kell elvégezni, de a valóságban, az nem megfelelően osztályozásához bármelyik nagyjövedelmű személyek (1 %).

Éppen ezért érdemes további mértékek, amely a kiértékelés szűkebb tulajdonságát rögzítése számítja ki. Mielőtt érkeznek e mértékek részleteit, fontos megérteni a fejetlenséget mátrix a bináris besorolás végzett kiértékelésre. Pozitív vagy negatív, a osztály címkék oktatás megadása a 2 csak lehetséges értékek, amely általában hivatkozunk vehet igénybe. A besorolás megfelelően = esetben előrejelzése pozitív és negatív-példányok úgynevezett igaz pozitív (Eszközpanel), és IGAZ negatív (TN), illetve. A helytelenül minősített példányok úgynevezett hasonlóan téves (FP) és a hamis negatív (FN). A fejetlenséget mátrix egyszerűen egy táblázatot 4 kategóriákkal alá tartozó példányok száma. Azure gépi tanulási automatikusan úgy dönt, amely a két osztályok az adathalmazban a pozitív eredetű. Logikai érték vagy az egészérték részt veszi figyelembe a osztály címkék esetén a "true" vagy "1" címkézett példányok a pozitív osztály vannak rendelve. Ha a címkék olyan karakterláncok, ahogy a bevétel adatkészletből lehetőséget választja, a címkék betűrend szerint vannak rendezve, és a negatív osztály lesz, amíg a második szintű eredetű pozitív választja az első szintű.

![Bináris besorolás Fejetlenséget mátrix](media/machine-learning-evaluate-model-performance/6a.png)

6 ábrán. Bináris besorolás Fejetlenséget mátrix.

Visszalépés a bevétel besorolás probléma, volna szeretnénk kérje meg a több értékelési kérdésekre, amelyek a segítse a szolgáltatás megértéséhez használt besorolás teljesítményét. Nagyon természetes kérdése van: "a személyek, akiknek a modell előre jelzett évi lehet, hogy ki > 50 K (Eszközpanel + FP), hány voltak besorolni helyesen (Eszközpanel)?" Ez a kérdés is megválaszolni megjeleníti, hogy helyesen sorolják pozitív aránya modell **pontosság** : TP/(TP+FP). Egy másik gyakori kérdés "kívül az összes magas évi bevétel az alkalmazottak > 50 k (Eszközpanel + FN), hány megváltozott a besorolás sorolják be helyesen (Eszközpanel)". Ez a ténylegesen a **visszahívása**vagy a true pozitív ráta: a besorolás a TP/(TP+FN). Megfigyelheti, hogy van-e egy egyértelmű csökkentés pontosság és a visszahívási között. Például egy viszonylag kiegyensúlyozott adatkészlet tekintve, amely = esetben előrejelzése a főként pozitív példányok besorolás szeretné, hogy egy nagy visszahívási, de a negatív példányok annyi meglehetősen alacsony pontosság volna misclassified téves nagyszámú így. Ha szeretné látni, hogyan változnak a következő két mértékek ábrázolása, a "PONTOSSÁG/visszahívási" görbe a kiértékelés kimeneti lapra, hogy kattint (a ábra 7 részének bal felső).

![Bináris osztályozási kiértékelés eredmények](media/machine-learning-evaluate-model-performance/7.png) ábra 7. Bináris osztályozási kiértékelés eredmények.

Egy másik kapcsolódó gyakran használt mérőszáma az **F1 pontszámhoz**, amely pontosság és a visszahívási figyelembe veszi. A harmonikus közép, a következő 2 mértékek, és ilyen számítja ki: F1 = 2 (visszahívási pontosság x) / (pontosság + visszahívási). Az F1 pontszámhoz összesítésére olyan egy számot a kiértékelés jó módszer, de tanácsos mindig egy is megtekintheti a pontosság és a visszahívási együtt, hogy jobban megértheti besorolás viselkedését.

Ezenkívül egy megvizsgálhatja az IGAZ pozitív rátát és a **Címzett működő jellemző (ROC)** görbe és a megfelelő **Terület csoportban a görbe (AUC)** értéket a hamis pozitív gyakoriság összehasonlítása. A közelebb a görbét, ha a bal felső sarokban, annál hatékonyabb a besorolás teljesítmény (, amely van maximalizálása az IGAZ pozitív ráta közben a kis méretre a hamis pozitív ráta). A görbék, amelyek a átlós ábra, eredménye, amely általában az, hogy az előrejelzések, amelyek közelébe véletlen találgatás osztályozónak közelébe.

###<a name="using-cross-validation"></a>Adatérvényesítési tartományok használata###
A regressziós példának megfelelően többször képzése, pontszámhoz, és az adatok másik részhalmazának automatikusan felmérése közötti érvényességi végezheti azt. Hasonlóképpen, ábrázolásakor a [Modell határokon érvényesítése] [ cross-validate-model] modul, egy kellő logisztikus regresszió modell és adatkészletet. *Bevétel* a [Modell határokon érvényesítése] kell beállítani a címke oszlop[ cross-validate-model] modul tulajdonságait. Miután a kísérlet fut, és kattintson a jobb oldali kimeneti port a [Modell határokon ellenőrzése] a[ cross-validate-model] modul, azt láthassa az egyes a nyíló metrikus bináris besorolás értékének ezeken az értéknél és szórásnál az egyes. 
 
![Bináris besorolás modell határokon érvényesítése](media/machine-learning-evaluate-model-performance/8.png)

Ábra 8. Bináris besorolás modell határokon érvényesítése.

![Idegen-ellenőrzési eredményeinek bináris besorolás](media/machine-learning-evaluate-model-performance/9.png)

Ábra 9. Bináris besorolás határokon-ellenőrzési eredményeinek.

##<a name="evaluating-a-multiclass-classification-model"></a>Multiclass besorolás modell értékelése##
Ez a kísérlet az fogjuk használni a népszerű [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") adatkészlet 3 különböző típusú (osztályok) a iris növény előfordulását tartalmazó. 4 szolgáltatás értékek is (sepal hossza és szélessége és szirom hosszúság/szélesség) az egyes példányok. Az előző kísérletek azt képzett, és tesztelje a modellek az azonos adatkészleteket használatával. Ebben az esetben fogjuk használni a [Szétválasztott adatok] [ split] modult, és hozhat létre 2 részhalmazának az adatokat, láthatja el ismeretekkel a az első, és pontszám a második felmérése. Az Iris adatkészlet a [UCI gépi tanulási tárházba](http://archive.ics.uci.edu/ml/index.html)nyilvánosan elérhető, és a az [Adatok importálása] letölthető[ import-data] modulra.

###<a name="creating-the-experiment"></a>A kísérlet létrehozása###
A következő modulokat hozzáadása az Azure gépi tanulási Studio a munkaterületet:

- [Adatok importálása][import-data]
- [Multiclass döntés erdő][multiclass-decision-forest]
- [Adatok felosztása][split]
- [Vonaton modell][train-model]
- [Pontszámhoz modell][score-model]
- [Modell felmérése][evaluate-model]

Csatlakozás a portokat, ábra 10 alább látható módon.

A címke oszlopindex [Vonaton modell] beállítása[ train-model] modul 5-ig. Az adathalmazban nincs rovatfej rendelkezik, de tudjuk, hogy az osztály feliratai az ötödik oszlopban.

Kattintson az [Adatok importálása] [ import-data] modul, és állítsa *A HTTP Protokollon keresztül, a webhely URL-címe*és http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data *URL-CÍMÉT* a *adatforrás* tulajdonságot.

Azon részét, a [Szétválasztott adatok] oktatás használandó példányok beállítása[ split] modul (például 0,7).
 
![Multiclass besorolás értékelése](media/machine-learning-evaluate-model-performance/10.png)

Ábra 10. Multiclass besorolás értékelése

###<a name="inspecting-the-evaluation-results"></a>A kiértékelés eredmények vizsgálata###
Futtassa a kísérlet, majd kattintson a kimeneti port [Felmérése]modell[evaluate-model]. A kiértékelés eredmények ebben az esetben egy fejetlenséget mátrix formájában mutatnak. A mátrix jeleníti meg a tényleges és a 3 összes osztály előre jelzett példányok.
 
![Multiclass osztályozási kiértékelés eredmények](media/machine-learning-evaluate-model-performance/11.png)

Ábra 11. Multiclass osztályozási kiértékelés eredmények.

###<a name="using-cross-validation"></a>Adatérvényesítési tartományok használata###
A korábban említett hajthatja végre ismételt oktatás, pontozási és használatával automatikusan a [Modell határokon érvényesítése] értékelést[ cross-validate-model] modulra. Egy adatkészletet, egy kellő modell és [Idegen érvényesítése modell] kimutatásadatokat[ cross-validate-model] modul (lásd az alábbi ábra). Újra kell beállítania a címke oszlopot a [Modell határokon érvényesítése] [ cross-validate-model] modul (oszlopindex 5 ebben az esetben). A kísérlet fut, és kattintson a jobb kimeneti port a [Modell határokon ellenőrzése]után[cross-validate-model], megvizsgálhatja az egyes nyíló, valamint a középérték és a normál eltérés metrikus értékeket. Az itt látható mértékek hasonlóak az abban az esetben, bináris besorolás tárgyalt szerint a lehetőségekből. Jó helyen jár, ne feledje, hogy multiclass besorolás, a true pozitív/negatív és a hamis pozitív/negatív számítások végzi osztály alapon, hogy nincs teljes pozitív vagy negatív osztály nem. Ha például a pontosság vagy a "Iris-setosa" osztály visszahívási számításakor feltételezett értéke, hogy ez legyen a pozitív osztály és összes többi negatív.
 
![Idegen-Multiclass besorolás modell érvényesítése](media/machine-learning-evaluate-model-performance/12.png)

Ábra 12. Multiclass besorolás modell határokon érvényesítése.


![Idegen-ellenőrzési eredményeinek Multiclass besorolás modell](media/machine-learning-evaluate-model-performance/13.png)

Ábra 13-mal. Idegen-ellenőrzési eredmények Multiclass besorolás modell.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
